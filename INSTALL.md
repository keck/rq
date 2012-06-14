=== Rubygems

The current version of rq runs on ruby1.8. A gem is available on rubygems:

  https://rubygems.org/gems/rq-ruby1.8

simply install 

  sqlite2 (Debian apt-get install libsqlite0-dev)

and

  gem1.8 install rq-ruby1.8

The gem includes sqlite-1.3.1 for Ruby. And should include:

  - gem1.8 install posixlock
  - gem1.8 install arrayfields
  - gem1.8 install lockfile

  these gems are also available from http://bio4.dnsalias.net/download/gem/ruby1.8/

Find rq

  gem1.8 contents rq-ruby1.8|grep bin/rq$

and add to the path, for example 

  /var/lib/gems/1.8/bin 
  
Maybe link it

  ln -sf `gem1.8 contents rq-ruby1.8|grep bin/rq$` /usr/local/bin/rq

  rq --help

Run the integration tests, e.g.

  test_rq.rb or /var/lib/gems/1.8/bin/test_rq.rb 

=== Debian

On Debian systems, the recommended procedure is to use Debian apt on all 
machines. For now, use the gem install, as documented above, and in the README!

This has been tested on Debian Squeeze (BioLinux Minimal):

        apt-get install libsqlite0-dev
          Setting up libsqlite0 (2.8.17-6) ...
          Setting up libsqlite0-dev (2.8.17-6) ...

        gem1.8 install rq-ruby1.8
          Building native extensions.  This could take a while...
          Successfully installed posixlock-0.0.1
          Successfully installed arrayfields-4.7.4
          Successfully installed lockfile-1.4.3
          Successfully installed rq-ruby1.8-3.x.x
          4 gems installed
          Installing ri documentation for posixlock-0.0.1...
          Installing ri documentation for arrayfields-4.7.4...
          Installing ri documentation for lockfile-1.4.3...
          Installing ri documentation for rq-ruby1.8-3.x.x...
          Installing RDoc documentation for posixlock-0.0.1...
          Installing RDoc documentation for arrayfields-4.7.4...
          Installing RDoc documentation for lockfile-1.4.3...
          Installing RDoc documentation for rq-ruby1.8-3.x.x...

      ln -sf `gem1.8 contents rq-ruby1.8|grep bin/rq$` /usr/local/bin/rq

    run

      test_rq.rb or /var/lib/gems/1.8/bin/test_rq.rb 

      rq --help

root@vagrant-debian-squeeze:/home/vagrant# rq --help
  NAME

    rq v3.x.x

(...)

which works on on 32-bits and 64-bits systems. The commands

  * rq ~/queue create
  * rq ~/queue status

should show

        jobs: 
          pending: 0
          holding: 0
          running: 0
          finished: 0
          dead: 0
          total: 0
        temporal: {}
        performance: 
          avg_time_per_job: 00h00m00.00s
          n_jobs_in_last_hrs: 
            1: 0
            12: 0
            24: 0
        exit_status: 
          successes: 0
          failures: 0
          ok: 0

=== RPM

Much like the Debian install, but you will need

  yum -y install ruby-devel rubygems sqlite2-devel
  gem install rq-ruby1.8

note Ruby 1.8.5 bombs out with 

  *** buffer overflow detected ***: /usr/bin/ruby terminated
  ======= Backtrace: =========
  /lib64/libc.so.6(__chk_fail+0x2f)[0x33972e6e1f]
  /usr/lib64/ruby/1.8/x86_64-linux/syck.so(rb_syck_mktime+0x48e)[0x2acb6522f08e]

you need Ruby 1.8.7! Probably a good idea to introduce RVM!

=== RVM

See https://rvm.io/rvm/install/ for installing Ruby 1.8.7. With rvm installed

  source /usr/local/rvm/scripts/rvm
  rvm install 1.8.6
  rvm use 1.8.6
  ruby -v
  gem env
  gem install rq-ruby1.8



=== NFS central

The second option is to build and install once and distribute through
NFS. Use the tgz installation, which includes an ./all/ directory. On 
modern systems your mileage may vary as the Ruby build breaks on gcc 
version >4.4

  * Unpack rq-ver.tgz file
  
  * cd into ./all/

  * ./install.sh /full/path/to/a/nfs/mounted/directory/

  * the nfs mounted path above should be visible by all cluster nodes.
    __all__ required software will be installed into this directory root. when
    complete all that's needed is a

      export PATH=/full/path/to/a/nfs/mounted/directory/bin:$PATH

    (note 'bin') to use rq

  * this is the second best procedure since it will result in a single nfs
    install which all cluster nodes can use. The other install methods mean
    you will have to install rq on __each__ node you plan to use it on.

=== STANDARD

(in rq version <=3.4.0)

  * install all packages in ./depends/packages manually

  * ruby install.rb

=== From source (github)

The current version of rq runs on ruby1.8, using bundler and jeweler:

  gem1.8 install bundler
  Successfully installed bundler-1.0.15

  gem1.8 install jeweler
  Successfully installed jeweler-1.6.4
 
  git clone https://pjotrp@github.com/pjotrp/rq.git
  cd rq/
  rake gemspec
  rake build  # creates gem in ./pkg

== Trouble shooting

=== rubyio.h error

Building native extensions.  This could take a while...
ERROR:  Error installing rq-ruby1.8-3.x.x.gem:
        ERROR: Failed to build gem native extension.

/usr/local/include/ruby-1.9.1/ruby/backward/rubyio.h:2:2: warning: #warning use "ruby/io.h" instead of "rubyio.h"

Solution: you are trying to build against ruby1.9. Use gem1.8 instead. If you 
have problems mixing gems, take a look at 'rvm'.

=== can not find rq, after successful install

gem1.8 stores gems in dirs named /var/lib/gems/1.8/gems/rq-ruby1.8-3.x.x/. With
'binaries' in /var/lib/gems/1.8/bin. Either add that to the path, or 
create a symbolic link, e.g.

  ln -sf `gem1.8 contents rq-ruby1.8|grep bin/rq$` /usr/local/bin/rq

=== can not find mkmf

root@vagrant-debian-squeeze:/home/vagrant# gem1.9.1 install mkmf
ERROR:  Could not find a valid gem 'mkmf' (>= 0) in any repository

Solution: install the development version of Ruby, for example ruby1.9-dev
on Debian.

