# Overview

This is code for our reproduction of Kademlia and its performance testing. The Mininet and Vagrant infrastructure portions
of the code were heavily inspired by or taken from the Jellyfish Reproduction
project for CS 244. The repo for that project is [here](https://github.com/brucespang/jellyfish-reproduction).

___Everything after this point from Jellyfish Repo README___

It provides a vagrant box with most of the tools you will need installed, and some structure for the project.

```

## Getting Started

1. Install [git](https://git-scm.com/)
1. Install [Virtualbox](https://www.virtualbox.org/), tested with 6.0.18.
1. Install [Vagrant](https://www.vagrantup.com/), tested with Vagrant 2.2.7
1. Open up a terminal
1. Clone this repo, `$ git clone https://github.com/brucespang/jellyfish-reproduction.git`
1. `$ cd jellyfish-reproduction`
1. Run `$ vagrant up` to create your Vagrant box. This takes some time--it will download a VM from vagrant and set it up. For me, it took about 3 minutes.
1. Run `$ vagrant ssh` to log into the newly created box.

You should see something like this:
```
$ vagrant ssh
...
vagrant@ubuntu-bionic:~$ ls
mininet  pox
vagrant@ubuntu-bionic:~ ls /vagrant
figures  jellyfish  Makefile  notebooks  README.md requirements.txt  setup.py	tests  Vagrantfile
```

All the code in the `jellyfish-reproduction` repo should be available in the `/vagrant` directory in the virtual machine. You should be able to edit files on your host machine using your favorite editor, and vagrant will automatically sync them.

### Generating figures

There are placeholder methods in `jellyfish/figures.py` for each figure you'll need to implement. Once implemented, you can generate a particular figure by runnning (e.g.):
```
vagrant@ubuntu-bionic:/vagrant$ jellyfish figure_1c figures/figure_1c.png
```

You can generate all the figures by running
```
vagrant@ubuntu-bionic:/vagrant$ make figures
jellyfish figure_1c figures/figure_1c.png
jellyfish figure_2a figures/figure_2a.png
jellyfish figure_2b figures/figure_2b.png
jellyfish figure_9 figures/figure_9.png
jellyfish figure_1c_mininet figures/figure_1c_mininet.png
jellyfish table_1 figures/table_1.txt
```

### Running Jupyter

[Jupyter](https://jupyter.org/) lets you run interactive Python notebooks, in which you can write python and draw graphs inline. We find that it dramatically improves our productivity over running a stand-alone script, especially if it takes a while to generate the data and we're making slight changes to graphs.

You are welcome to work on the project however you want, but if we were working on this project, we would make all the graphs in Jupyter and then move the code to `jellyfish/figures.py`. This would help get each graph done faster, and then we would be able to easily test and generate all the figures before turning in the project.

You can start the jupyter notebook server by running the following. There's an example notebook in `notebooks/Example Notebook.ipynb`

```
vagrant@ubuntu-bionic:/vagrant$ jupyter notebook --ip=0.0.0.0
```

You should be able to access the notebook server from your *host* computer using one of the URLs. For me, the url looks like [http://127.0.0.1:8888/?token=40d8fdab98ce2be96ac5cafa6ed610a509b7e5c977c60fd1](http://127.0.0.1:8888/?token=40d8fdab98ce2be96ac5cafa6ed610a509b7e5c977c60fd1) but your token will be different.

*Note:* This works by forwarding port 8888 on the host machine to port 8888 on the local machine, and is configured in the Vagrantfile. It's important that you use the `--ip=0.0.0.0` so that Jupyter listens on all interfaces and that the port forwarding works.

### Running mininet

There's some placeholder code in `jellyfish/mininet.py` you will need to fill in which turns the graph you generated in part 2 of the assignment into a mininet topology. Once you have, you can start mininet by running
```
vagrant@ubuntu-bionic:/vagrant$ sudo jellyfish mn --graph='fat_tree' -k 4
...
*** Starting CLI:
mininet>
```

If you want to play around with mininet, it is installed in the usual way. You can do things like:
```
vagrant@ubuntu-bionic:/vagrant$ sudo mn
*** Creating network
*** Adding controller
*** Adding hosts:
h1 h2
*** Adding switches:
s1
*** Adding links:
(h1, s1) (h2, s1)
*** Configuring hosts
h1 h2
*** Starting controller
c0
*** Starting 1 switches
s1 ...
*** Starting CLI:
mininet> pingall
*** Ping: testing ping reachability
h1 -> h2
h2 -> h1
*** Results: 0% dropped (2/2 received)
```

### Running tests

There are some very basic tests written for your Jellyfish graph generator in `tests/`. You will also need to write your own routing test. You can run the files on their own, or run them all together by running
```
vagrant@ubuntu-bionic:/vagrant$ make test
test_correct_degree (tests.test_jellyfish.TestJellyfishGenerator) ... ok
test_correct_number_hosts (tests.test_jellyfish.TestJellyfishGenerator) ... ok
test_correct_number_switches (tests.test_jellyfish.TestJellyfishGenerator) ... ok
test_jellyfish_hosts_reachable (tests.test_routing.TestRouting) ... ok

----------------------------------------------------------------------
Ran 4 tests in 1.191s

OK
```