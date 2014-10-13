Apt sources
=========

This role can be used to manage Apt package sources. It supports both regular
Debian and Ubuntu's PPA sources. The role provides an easy way to only pick the
packages you want to take priority over the ones in the base release via Apt
pinning.

Requirements
------------

python-apt has to be present on the server.

Role Variables
--------------

      apt_source_url: http://packages.dotdeb.org

The URL of the package source.

      apt_source_components: all

List of components (such as "main contrib") from the source.

     apt_source_distribution: "{{ansible_lsb.codename}}"

The distribution (such as "wheezy") of the package source. Usually the default of
ansible_lsb.codename will give desired results.

      apt_source_source_string: ppa:nginx/stable

The source string can be defined by setting this variable. This is used instead
of apt_source_url, apt_source_components etc. when adding a PPA.

      apt_source_key:
        url: http://www.dotdeb.org/dotdeb.gpg
        id: 89DF5277

A dict containing the URL and ID of the public key associated with the source
as accepted by the apt_key module.

      apt_source_packages:
        - pattern: '*nginx*'

A list of patterns for packages that should get the priority defined by
apt_source_high_priority. This for making the desired packages the primary
candidate for installation by apt.

     apt_source_source_selector: "release o={{apt_source_release_origin}}"

This is the selector used when pinning packages. The role tries to set
reasonable defaults based on the other parameters but set this if needed. For
example the varnish repos need this to be set to o=varnish-cache.org while the
default value is o=repo.varnish-cache.org. Check with apt-cache policy.

     apt_source_state: absent

To remove a package source, set this parameter.

     apt_source_high_priority: 500

The priority set for packages that are picked from the source with
apt_source_packages.

     apt_source_default_pin:
       - pattern: "*"
         priority: 200

The default pin priority set for all packages in the source. You could change
this to pin them all to a different priority.

Example Playbook
----------------

    - hosts: servers
      roles:
          apt_source_url: http://packages.dotdeb.org
          apt_source_components: all
          apt_source_key:
            url: http://www.dotdeb.org/dotdeb.gpg
            id: 89DF5277

License
-------

MIT/Simplified BSD license

Author Information
------------------

Role created by Antti J. Salminen in 2014.