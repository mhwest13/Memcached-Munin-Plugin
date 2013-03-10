Memcached Munin Plugin
============================

Munin plugin for graphing information out of Memcached

Memcached: http://memcached.org

Munin: http://munin-monitoring.org


What does this plugin do?

Well depending on the version you are running, this plugin will give you either a bird's eye view of a node's Memcache performance,
or the bird's eye view as well as the worm's view... it breaks down hit rates / evictions and other metrics to a per slab basis.
Ultimately this would allow a power user to re-tune their Memcache cluster to better handle slabs of a certain size.


PLUGINS
-------
Available Plugins:

### memcached_multi_
  * bird's eye overview, and the worm.  
    + Requires Munin Master/Client v1.4.x+ due to multigraph requirements.

### memcached_
  * bird's eye overview
    + Built for munin v1.2.x and non-power users

INSTALLATION
------------

1) Dependencies  
  * munin-master v1.4.+  
  * munin-node v1.4.+  
  * memcached  
    + v1.4.0+ for memcached_multi_ (last tested / coded against v1.4.15)  
    + v1.2.6+ for memcached_  
  * Perl Modules: IO::Socket, File::Basename  

2) Make sure the plugin lives in /usr/share/munin/plugins/ as memcached_multi_ or memcached_  
  * This of course depends on which plugin you have downloaded.  
  * The plugin must be called up using symlinks so it knows which graph your are trying to fetch information for.  

3) Inform Munin of plugin dependencies, change options as necessary in the files located in: /etc/munin/plugin-conf.d  

### memcached_multi_  
    /etc/munin/plugin-conf.d/memcached_multi  

    [memcached_multi_*]  
    env.host 127.0.0.1  
    env.port 11211  
    env.timescale 3  
    env.cmds get set delete incr decr touch  
    env.leitime -1  

### memcached_  
    /etc/munin/plugin-conf.d/memcached  

    [memcached_*]  
    env.host 127.0.0.1  
    env.port 11211  
    env.timescale 3  

4) Let's see if munin can detect and use the plugin with its internal calls.

### memcached_multi_

    munin-node-configure --suggest | grep memcached_multi_

Should return something similar to this if everything checks out...

    memcached_multi_ | no | yes (bytes commands conns evictions items memory unfetched)

### memcached_

    munin-node-configure --suggest | grep memcached_

Should return something similar to this if everything checks out...

    memcached_ | no | yes (bytes commands conns evictions items memory)

5) Let's see what munin thinks our symlinks should look like for plugin we are attempting to execute.

### memcached_multi_

    munin-node-configure --suggest --shell | grep memcached_multi_

Should return this or similar results if everything checks out...

    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_bytes'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_commands'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_conns'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_evictions'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_items'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_memory'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_unfetched'

### memcached_

    munin-node-configure --suggest --shell | grep memcached_

Should return this if everything checks out...

    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_bytes'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_commands'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_conns'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_evictions'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_items'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_memory'

6) Be sure to restart munin-node so it can leverage the new plugins.


INFORMATION
-----------
If you'd like to learn further about the plugin, there is more documentation stored in the
file as POD, you can view this info with perldoc.

Source Code < https://github.com/mhwest13/Memcached-Munin-Plugin >

Issues < https://github.com/mhwest13/Memcached-Munin-Plugin/issues >

Further Help < http://munin-monitoring.org/wiki/HowToGetHelp >
