Memcached Munin Plugin
============================

Munin plugin for graphing information out of Memcached

Memcached: http://memcached.org

Munin: http://munin-monitoring.org


## What do these plugins do?

 Depending on the version you are running, you have the possibility for extra debugging information to be exposed.  
  * Both of the plugins expose the global memcached statistical information.  
    
    * [memcached_](README.md#memcached_) ( No real extra features, may not always receive constant updating )  
    * [memcached_multi_](README.md#memcached_multi_) ( Provides hit rates / evictions / other memory information on a per slab basis )  

  * Ultimately the difference between them is like comparing only having a bird's eye view of a node's performance
    or the bird's eye view, as well as the worm's view.
    * [memcached_](README.md#memcached_) is meant more for Munin v1.2.x and/or Memcached 1.2.x / Non-Power users.
    * [memcached_multi_](README.md#memcached_multi_) allows for more granular information to be collected and stored over time, so you can watch the
      consumption of memory and re-tune the slabs to better handle the workload of the application. Although it does require
      Munin v1.4.x+ to be running on both the Master and Node, this is in order to support multigraph capabilities.


## "Plugin Prefixing"

  **This is an advanced topic, pay no attention to this unless you plan on monitoring more than 1 memcached instance from a single munin node**

  Both plugins support prefixing when creating a symlink to the plugin from the common munin plugin directory.
  When you create the plugin, the portion you prefix the symlink with will be used in the graph title, and the
  first letter of the prefix will be capitalized. This allows for configuring different memcached monitoring
  from a single host, because you can override settings by using the plugin-conf.d files.

    For instance:
        Symlinks that ultimately exposed themselves like the following, would prefix all of the graphs
        for this memcached_multi_ display with Global, as well as use the settings defined for
        [global_memcached_multi_*] in your munin plugin-conf.d file, assuming they are defined, otherwise
        the default environment variables will be used.

        /etc/munin/plugins/global_memcached_multi_bytes     -> /usr/share/munin/plugins/memcached_multi_
        /etc/munin/plugins/global_memcached_multi_commands  -> /usr/share/munin/plugins/memcached_multi_
        /etc/munin/plugins/global_memcached_multi_conns     -> /usr/share/munin/plugins/memcached_multi_
        /etc/munin/plugins/global_memcached_multi_evictions -> /usr/share/munin/plugins/memcached_multi_
        /etc/munin/plugins/global_memcached_multi_items     -> /usr/share/munin/plugins/memcached_multi_
        /etc/munin/plugins/global_memcached_multi_memory    -> /usr/share/munin/plugins/memcached_multi_
        /etc/munin/plugins/global_memcached_multi_unfetched -> /usr/share/munin/plugins/memcached_multi_


INSTALLATION
------------

### memcached_multi_

1) Dependencies
  * Munin ( master v1.4.x+ )
  * Munin ( node v1.4.x+ )
  * Memcached v1.4.0+ ( last tested / coded against v1.4.15 )
  * Perl Modules: IO::Socket, File::Basename

2) Make sure the plugin lives in your munin plugin directory as memcached_multi_
  * The plugin must be called up using symlinks so it knows which graph your are trying to fetch information for.
  Example: /usr/share/munin/plugins/memcached_multi_

3) Inform Munin of plugin dependencies, change options as necessary in the files located in: /etc/munin/plugin-conf.d
  * This is also where you would define additional environment settings for prefixed plugins. Multiple definitions can
    be supplied in the same file for differently named plugins.

    /etc/munin/plugin-conf.d/memcached_multi_

    [memcached_multi_*]
    env.host 127.0.0.1
    env.port 11211
    env.timescale 3
    env.cmds get set delete incr decr touch
    env.leitime -1

4) Let's see if munin can detect and use the plugin with its internal calls.

    munin-node-configure --suggest | grep memcached_multi_

Should return something similar to this if everything checks out...

    memcached_multi_ | no | yes (bytes commands conns evictions items memory unfetched)

5) Let's see what munin thinks our symlinks should look like for plugin we are attempting to execute.

    munin-node-configure --suggest --shell | grep memcached_multi_

Should return this or similar results, depending on your version, if everything checks out...

    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_bytes'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_commands'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_conns'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_evictions'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_items'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_memory'
    ln -s '/usr/share/munin/plugins/memcached_multi_' '/etc/munin/plugins/memcached_multi_unfetched'

6) Be sure to restart munin-node so it can leverage the new plugins.


### memcached_

1) Dependencies
  * Munin ( master v1.2.x+ )
  * Munin ( node v1.2.x+ )
  * Memcached v1.2.6+ ( last tested / coded against v1.2.6 )
  * Perl Modules: IO::Socket, File::Basename

2) Make sure the plugin lives in your munin plugin directory as memcached_multi_
  * The plugin must be called up using symlinks so it knows which graph your are trying to fetch information for.
  Example: /usr/share/munin/plugins/memcached_

3) Inform Munin of plugin dependencies, change options as necessary in the files located in: /etc/munin/plugin-conf.d
  * This is also where you would define additional environment settings for prefixed plugins.

    /etc/munin/plugin-conf.d/memcached

    [memcached_*]
    env.host 127.0.0.1
    env.port 11211
    env.timescale 3

4) Let's see if munin can detect and use the plugin with its internal calls.

    munin-node-configure --suggest | grep memcached_

Should return something similar to this if everything checks out...

    memcached_ | no | yes (bytes commands conns evictions items memory)

5) Let's see what munin thinks our symlinks should look like for plugin we are attempting to execute.

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
