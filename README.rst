==========================
MUNIN Plugin for MEMCACHED
==========================

`memcached <http://memcached.org>`_ plugin for `Munin <http://munin-monitoring.org>`_

-------
PLUGINS
-------

There are two plugins available in this repository. You should choose one based on your needs:

1. memcached_  - High-level overview for munin v1.2.x+ and non-power users

2. memcached_multi_  Includes both high-level and more detailed views. Requires Munin Master/Client v1.4.x+ due to multigraph requirements.

It breaks down hit rates / evictions and other metrics to a per slab basis.

Ultimately this would allow a power user to re-tune their Memcache cluster to better handle slabs of a certain size.


------------
INSTALLATION
------------

1. Dependencies
    - munin-master v1.4.+
    - munin-node v1.4.+
    - memcached
        - v1.4.2+ for memcached_multi_
        - v1.2.6+ for memcached_
    - Perl Modules: IO::Socket, File::Basename

2. Make sure the plugin is located in `/usr/share/munin/plugins/` as memcached_multi_ or memcached_, depending on which you chose. (For ease of documentation, for the rest of these instructions we'll assume you're installing memcache_.) Ensure that the file is executable, e.g. `chmod +x`.

3. You will need to create six symlinks from `/usr/share/munin/plugins/memcache_` to `/etc/munin/plugins`:

    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_bytes'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_commands'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_conns'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_evictions'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_items'
    ln -s '/usr/share/munin/plugins/memcached_' '/etc/munin/plugins/memcached_memory'

4. Configure the plugin. This can either go in a separate config file such as in `/etc/munin/plugin-conf.d/memcached` or in a "master" config such as `/etc/munin/plugin-conf.d/munin-node`. For example (using all memcached defaults):

    [memcached_*]
    env.host 127.0.0.1  
    env.port 11211      
    env.timescale 3     

5. If you have installed and configured everything properly, test it by running:

    munin-node-configure --suggest | grep memcached_

If your setup is correct, you should see a line similar to:

    memcached_ | yes | yes (bytes commands conns evictions items memory)

6. To test that your symlinks are configured properly:

    munin-node-configure --suggest --shell | grep memcached_

If your symlinks are set properly, this should return no output. You may receive some stderr lines, but as long as they do not mention memcache_, your installation is fine. If your symlinks are not properly configured, you'll see lines exactly like those in step 3. You will need to create each of the symlinks that are printed to screen at this time.


7. Restart munin-node


-----------
INFORMATION
-----------

If you'd like to learn further about the plugin, there is more documentation stored in the file as POD. You can view this info with perldoc.

`Source Code <https://github.com/mhwest13/Memcached-Munin-Plugin>`_

`Issues <https://github.com/mhwest13/Memcached-Munin-Plugin/issues>`_

`Further Help <http://munin-monitoring.org/wiki/HowToGetHelp>`_
