Optional Features
=================

Enabling Optional Applications
------------------------------

To activate the optional applications, add these settings to your
``host_vars``:

-  ``flexget_enabled: yes`` for *FlexGet*.
-  ``rutorrent_enabled: yes`` for *ruTorrent*.

Read the following sections for details.


.. _flexget:

Installing FlexGet
------------------

After setting ``flexget_enabled: yes``, run the playbook again.

FlexGet is just installed ready to be used, for full operation a
configuration file located in ``~/.config/flexget/config.yml`` must be
added (see the `FlexGet cookbook`_). A cronjob is provided too (called
every 11 minutes), but only starts to actually call FlexGet *after* you
add that configuration file.

Look into the files
``~/.config/flexget/flexget.log`` and
``~/.config/flexget/flexget-cron.log`` to diagnose any problems.

.. _FlexGet cookbook: http://flexget.com/wiki/Cookbook


.. _rutorrent:

Installing and Updating ruTorrent
---------------------------------

The ruTorrent web UI is an optional add-on, and you have to activate it
by setting ``rutorrent_enabled`` to ``yes`` and providing a
``rutorrent_www_pass`` value, usually in your
``host_vars/my-box/main.yml`` and ``host_vars/my-box/secrets.yml``
files, respectively. Then run the playbook again.

Alternatively to the self-signed certificate that is created for Nginx,
you can also copy a certificate you got from other sources to the paths
``/etc/nginx/ssl/cert.key`` and ``/etc/nginx/ssl/cert.pem``.
See `this blog post`_ if you want *excessive* detail on secure HTTPS setups.

After the second run, ruTorrent is available at
``https://my-box.example.com/rutorrent/`` (use you own domain or IP in
that URL).

To *update to a new version* of ruTorrent, first add the desired version
as ``rutorrent_version`` to your variables – that version has to be
available on `Bintray`_. Then move the old installation tree away:

.. code-block:: shell

    cd ~rutorrent
    mv ruTorrent-master _ruTorrent-master-$(date "+%Y-%m-%d-%H%M").bak
    tar cfz _profile-$(date "+%Y-%m-%d-%H%M").bak profile

Finally, rerun the playbook to install the new version. In case anything
goes wrong, you can move back that backup you made initially.

.. _this blog post: https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html
.. _Bintray: https://bintray.com/novik65/generic/ruTorrent#files
