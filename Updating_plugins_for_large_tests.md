#How to run Pluginsync for large tests
The following steps are necessary for updating plugins to the new large test framework. 

###In the snap-pluginsync repo:
- Fork and clone repo of plugin you want to run large tests on
- Fork repo and clone snap-pluginsync
- Run `$ ./pluginsync.sh`
  - If this errors make sure ssh keys are set up
  
###In container:
- run `$ msync update -f {plugin_name} —noop`
- to exit container: `$ exit`
- cd into `modules/{plugin_name}`
- Commit changes and push pluginsync branch to your github repo
  - **If you plan to make a PR with this branch**, make sure you set the following variables in .travis.yml 
      
      `sudo: true
      services:
        docker`

- cd OUT of snap-plugignsync and INTO {plugin_name} <— The one you cloned earlier
- git fetch and git checkout the pluginsync branch that you just pushed 
- Run large tests with `$ make test-large`
  - To run in debug mode so you can examine the container while still running,
    run `$ DEMO=true make test-large`
  - To examine the container, in a separate terminal run 
    `$ docker exec -it $(docker ps | sed -n 's/\(\)\s*intelsdi\/snap.*/\1/p') /bin/bash`
  - To view Snap daemon log, in separate terminal run,
    `$ docker logs $(docker ps | sed -n 's/\(\)\s*intelsdi\/snap.*/\1/p’)`

- If your plugin has a large_tests.sh or something similar in its /scripts directory, delete that. It is nolonger necessary. 

FIN
