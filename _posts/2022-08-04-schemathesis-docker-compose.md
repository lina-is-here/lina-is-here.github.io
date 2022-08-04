---
layout: post
title:  "How to use Schemathesis to test Flask API in GitHub Actions"
date:   2022-08-04
tags: [schemathesis, api, testing, docker, docker-compose, github actions]
---

It's been definitely more than a year since I first discovered [Schemathesis](https://github.com/schemathesis/schemathesis).
I ran it against one API and got overwhelming amount of errors. Recently I re-discovered it again and something just clicked!


I started to implement fuzzy testing for [Ibutsu](https://github.com/ibutsu/ibutsu-server), which is basically an API written 
in Flask (python).


The application consists of several parts: frontend, backend, worker, postgres and redis. So how can we run it, run 
fuzzy testing against it and do it all in each PR in GitHub actions?


<!--more-->

 **1. First, let's create a docker-compose file with all the necessary components.**

For example, it doesn't make sense to run frontend, because it's not under test in fuzzy testing.

It's common, that the docker-compose file already exists, so we just need to make some adjustments in the new file.

 **2. Next, add the new container to docker-compose.**

Fuzzy testing will be performed from this container. I call it `api-tests` (see example in 
[Ibutsu repo](https://github.com/ibutsu/ibutsu-server/blob/master/docker-compose-fuzz.yml)). 
This container also requires a Dockerfile. The Dockerfile is very simple, it is an image with installed 
schemathesis, and a script that performs the testing, see [example](https://github.com/ibutsu/ibutsu-server/blob/master/backend/docker/Dockerfile.fuzz_testing).


The script should wait for the API to be available (otherwise it will immediately fail and container will exit) and 
run the test command. In the case of Ibutsu, it also acquires a valid token, so it can be later included in all 
requests, see [here](https://github.com/ibutsu/ibutsu-server/blob/master/backend/docker/start_fuzz_testing.sh).

 **3. Test it locally.**

Before proceeding to GitHub actions, let's verify it works as expected.

Run the new docker-compose configuration locally:

{% highlight bash %}
docker-compose -f docker-compose-fuzz.yml up
{% endhighlight %}

Verify it works as expected and tests are performed.

 **4. Once it works locally, create a GitHub workflow.**

In the root of your project, create a file `.github/workflows/api_test.yaml`.

The job does just two things: checks out the repo and then runs docker-compose. You can check how it works in 
[Ibutsu](https://github.com/ibutsu/ibutsu-server/blob/master/.github/workflows/api_tests.yaml).

 **5. Fix the errors!**

Everything should work by now, and the only part that could be "not perfect" is the API under test :D 


P.S. I also highly recommend going through [Schemathesis documentation](https://schemathesis.readthedocs.io/en/stable/) 
to find all the possible options and enhance your tests and API. Happy fuzzing!
