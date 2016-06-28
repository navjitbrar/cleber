#cleber Automation Engine
----
This is a simple expect based automation framework that enables talking with QEMU instances.
It has the concept of pluggins in which users can create a DUT (or SUT). The pluggins are called from the core engine, their spawn id managed and allowed to be communicated with from commands offered by the "cleber" engine.
Terminology leans more towards Agile / DevOps methodology where "stories" rule.

##Requires:
 A valid TCL/expect installation.

##Sample execution:
 `cleber --exp /local/expect --site_plugins /opt/cleber_plugins --plugins ~/plugins --config iotdev1:iotdev1 --cmds /opt/scripts/ulp_cmds.txt`
  OR
  `cleber --exp /local/expect --site_plugins /opt/cleber_plugins --plugins ~/plugins --config ~/myconfigs/iotdev-config --cmds /opt/scripts/ulp_cmds.txt`
 * Important point to note:
  * If the --config option points to a file, it will be prased, else, the config pattern should just be per following format:
  * <plugin>:<plugin>
 
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
cli 1 1 1  sudo su root
cli 1 1 1  1finity
cli 1 1 1  run show eqpt shelf pi
cli 1 1 1  set system ntp disabled
cli 1 1 1  commit

for { set i 10 } { $i < 20 } { incr i} {
    cli 2 1 1  set complete-on-space false
}

# Address cluster#2, Device#1, cli port#1
cli 2 1 1  set paginate false
cli 2 1 1  configure
cli 2 1 1  sudo su root
cli 2 1 1  1finity
cli 2 1 1  run show eqpt shelf pi
cli 2 1 1  set system ntp disabled
cli 2 1 1  commit

clock seconds
TESTCASE_END
STORY_END

```
For above to work, "iotdev1.sh" will need to exist in the ~/plugins or a site common directory (like /opt/cleber_plugins).
* Please note: For simpliciy, plugins are bash scripts.
* In example above, `iotdev1:iotdev1` translate to cluster 1 and 2 respectively.
* So, `cli 1 1 1` will address first `cli` port of the first card in first cluster.
* Similarly, `cli 2 1 1` will address first `cli` port of the first card in the second cluster.
* 


##plugin format:
* A bash script.
* Should provide a executable command-line in the end.
* Look for sample iotdev1 example in the `./plugins`, a subdirectory in this repository
 * After that, add your *plugin* to your local site page and socialize the location.
    
##Settings file:
 Much like .gdbinit, cleber has a `.cleberinit` file that exists in the user's home directory.
 * One can add the often used configuration parameters to this file.
 
 ```
 EXPECT=/local/expect
 SITE_CONFIG_DIR=/opt/cleber_plugin
 CONFIG=iotdev1:iotdev1
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
 
