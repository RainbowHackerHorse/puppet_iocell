# FreeBSD Jail management with Puppet, using iocell

[![Puppet Forge](https://img.shields.io/puppetforge/v/hackerhorse/iocell.svg)](https://forge.puppet.com/hackerhorse/iocell) [![Build Status](https://travis-ci.org/RainbowHackerHorse/puppet_iocell.svg?branch=master)](https://travis-ci.org/RainbowHackerHorse/puppet_iocell)

Manage FreeBSD jails with Puppet, leveraging [iocell] for jail management.

### Compatibility Warning

This module conflicts with zleslie/jail

### Setup

This module expects to be the only jail manager on a given system.  Each system where jails will be managed needs to include the `jail::setup` class as well.

```Puppet
include jail::setup
```

This simply installs 'iocell' and removes '/etc/jail.conf'.

This allows the type to use the correct jail without having to
specify the pool on each jail.

### A simple jail

```Puppet
jail { 'myjail1':
  ensure    => present,
  state     => 'up',
  ip4_addr  => 'em0|127.0.1.0/29',
  ip6_addr  => 'em0|fc00::10/64',
  hostname  => 'myjail1.example.com',
  boot      => 'on',
  pcpu      => '20'
  memoryuse => '4G',
  quota     => '15G',
  release   => '11.1-RELEASE'
  rlimits   => 'on',
  user_data => template('mysite/user_data.sh.erb'),
}
```

Note the `ip4_addr` and the `ip6_addr` properties take an interface name and an IP address separated by a pipe character.  This value is passed directly to `iocell(7)`.  You may wish to read the man page.

[iocell]: http://iocell.readthedocs.org/en/latest/

