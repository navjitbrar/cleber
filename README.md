#cleber Automation Engine
----
This is a simple expect based automation framework that enables talking with QEMU instances.
It has the concept of pluggins in which users can create a DUT (or SUT). The pluggins are called from the core engine, their spawn id managed and allowed to be communicated with from commands offered by the "cleber" engine.
Terminology leans more towards Agile / DevOps methodology where "stories" rule.

##Requires:
 A valid TCL/expect installation.

##Sample execution:
 `cleber --exp /local/expect --site_confdir /opt/configs --confdir ~/configs --config iotdev1 --cmds /opt/scripts/ulp_cmds.txt`
 
##ulp_cmds.txt:
```
  STORY "MY story"
  TESTCASE "My TC"
  clock seconds
  for { set i 10 } { $i < 20 } { incr i} {
      cli 1 1 1  set complete-on-space false
  }

  cli 1 1 1  set paginate false
  cli 1 1 1  configure

  for { set i 10 } { $i < 20 } { incr i} {
      cli 1 2 1  set complete-on-space false
  }

  cli 1 2 1  set paginate false
  cli 1 2 1  configure

  clock seconds
  TESTCASE_END
  STORY_END
```
For above to work, "iotdev1" will need to exist in the ./configs or ~/configs local or a site common directory (like /opt/configs).

##pluggin format:
* A bash script.
* Should provide a executable command-line in the end.
* Look for sample iotdev1 example in the `./configs`, a subdirectory in this repository
 * After that, add your configurations to your local site page and socialize the location.
    
##Settings file:
 Much like .gdbinit, cleber has a `.cleberinit` file that exists in the user's home directory.
 * One can add the often used configuration parameters to this file.
 
 ```
 EXPECT=/local/expect
 SITE_CONFIG_DIR=/opt/confdir
 CONFIG=iotdev1
 CMDS=/opt/scripts/ulp_cmds.txt
```
Then above execution could be simplified to:
 `cleber`
 
##Commands:
```
 cli <cluster> <card> <port> "command"
 dip <cluster> <card> <port> "command"
 ```
 Right now, it supports, CISCO cli style prompts.
 
