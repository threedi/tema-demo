# Original proposed setup from april 2024

My original idea below could be simplified, but I've retained the documentation as it might be handy in the future.

## Setup

The sketch below shows the suggested setup, followed by the textual explanation of the various parts. Basically we use the context broker in combination with a custom vocabulary + three **small and simple** dockers inside the K3S cluster + our own S3 storage for shuttling info back and forth.

![Sketch of the suggested setup](suggested-setup.png)


## Context broker: custom vocabulary

In the context broker, we'll need to store information. Information that needs terms and definitions because of its "linked data" nature. So if we want to talk about a `RegionAtRisk`, we'll need to define.

If there's already a suitable vocabulary, we can use it. But at first we're probably better off making our own "JSON-LD" vocabulary for our specific use case. That way we can store `https://our.vocabulary.org/v1/meta/RegionAtRisk=Ahrtal` in the context broker and add a subscription to that.

We'll start such a bare-bones vocabulary and host it somewhere public. If handy, others can extend it or start their own.

Some initial ideas (all with proper urls instead of just words, of course):

    weather-occurrence
        id=abcd
        region = ahr
        timestamp = 2021-07-xx
        rainfall-data = s3://amaazon.com/xxx/something.tiff

    flood-calculation
        weather-occurrence=http://tema.it/weather-occurrence/abcd
        result=s3://amazon.com/s3/xxx/something.zip

    disturbance
        3di-scenario=https://www.3di.live/scenario/xyz
        height=235
        x1=...
        y1=...
        x2=...
        y2=...


## K3S part 1a: *context broker* target 'flask' docker

You can subscribe at the context broker for certain specific items. For instance a new `RegionAtRisk`. You have to provide a URL that the context broker can send its notification to.

We'll make a docker that contains a small [flask (python)](https://pypi.org/project/Flask/) webserver that serves as the "target URL" for the context broker. Apparently there already is a small example app available within the project, but I've only seen the filenames in a screenshot :-)

There's the technical detail of subscribing. It depends on the behaviour of the context broker. Perhaps we can just re-register ourselves when the flask server starts up?

(The flask docker will be registered through a kubernetes `deployment` yaml file. The details will have to be in some non-public repository, but once it works we'll probably put an example here.)


## K3S part 1b: what the 'flask' docker does

Custom scripting, installs of your own software: that's best handled on your own servers. At least, that's my line of thinking.

So the 'flask' docker does this:

- It gets the notifications from the context broker.
- It converts them into some json instruction.
- It uploads it to a company-specific S3 object store in an `inbox/` directory.
- (Afterwards, our custom software will look in that directory and start up 3Di simulations and so).


## K3S part 2: 'cronjob' script docker

The next part will be a docker running some python script. It is started as a kubernetes `cronjob` resource, which means it is simply run once in a while (every minute for instance).

The simple script will probably do this:

- Look in our company-specific S3 object store in the `outbox/` directory.
- Any messages/objects it finds there are posted to the context broker.

Such messages can be things like "area to really watch with drones".

This way, whatever we do or calculate or determine can be made available through the context broker. There is a possibility of storing blobs of data in an internal "minio" object store, but our guess is that it is handier to just provide the URL to our s3 store for the results we've calculated, as that will be a non-firewalled URL, btw.


## K3S part 3: status overview docker

To make the process visible/observable, we propose a simple web interface that shows the current status from our point of view:

- It downloads a `status.json` from our company-specific S3 object store.
- It shows what "our" infrastructure has received, what is being processed and what is ready.

This docker is deployed as a `deployment` resource with a matching `service` and `ingress`. (In normal terms: a website with a url).


## Summary

Our main focus is improving the actual "3Di" flood calcuation software. What I'm describing here is what we (and probably lots of other partners) need to do to cooperate:

- Find or make a good vocabulary for adding what we need to the *context broker*.
- A webserver that the context broker can send targeted messages to.
- A script that feeds the context broker (and possibly the META minio storage) with updated content.


## Possible case study

I'm throwing in [a possible case study](example-case-study/case-study.md) just to make the possible data flows a bit more concrete.
