slaprunner
==========

Introduction
------------

This software release is used to deploy Slaprunner instances.
Slaprunner is an all-in-one IDE used to develop and test profiles and recipes for SlapOS.

Parameters
----------

authorized-key
~~~~~~~~~~~~~~

You need to specify your SSH public key in order to connect to the SSH server of this instance.

Example of parameter XML::

  <?xml version="1.0" encoding="utf-8"?>
  <instance>
  <parameter id="authorized-key">ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCdNp7qZMVAzNc07opHshiIFDmJpYXQpetfcSgUj39a409d42PpsJElp7WsAE/x0nN6gUIoWIl7UiAlMzf6bKEJGJVSOZEPTmiJVlgK1Gp+kE0x9yNcncYg7p38Jny0daVA/NkkpAFyRsAm5kLGzyLtaCcktSvy0cJuy7WSSHU05pd1f8Y8thofE9g5t+/JA2VZvipxPkRfkFAG3aOAGLULlTImTSDFSDFGSDFG5F6mMnl7yvY2d6vEHVBu+K+aKmAwZVfCUwtSpa/tq3i2Lppjrw3UfrxbQSFHZCkzefr+u+l4YYe+tJrX7rYJYXD7LIfZfdSeFTlHFaN/yI1 user@host.local</parameter>
  </instance>

instance-amount
~~~~~~~~~~~~~~~

Optional parameter.

Default: 10

AUTO-DEPLOYMENT
---------------

You can automatically deploy a software release while deploying the webrunner itself, using the paramater XML.

To do this, you only need to pass as a parameter named "slapos-software" : "AAA/BBB", where AAA is the folder of slapos.git where is located your BBB software.
For example, to deploy the hello-world software, you need to pass : software/helloworld

This is possible because the slapos.git is automatically downloaded when the webrunner is deployed.

It is also possible to download you own git repository, by providing the url in the "slapos-repository" parameter.

Last but not least, it is also possible to switch the branch with the parameter "slapos-reference" (by default pointing on master)

Resilience :
------------

To order a resilient runner, you have to request a runner with the option: "--type resilient"

You can then decide on which node you want to deploy each instance, with the parameters.xml :
	<?xml version='1.0' encoding='utf-8'?>
	<instance>
	  <parameter id="-sla-runner2-computer_guid">COMP-XXXX</parameter>
	  <parameter id="-sla-pbs2-computer_guid">COMP-XXXX</parameter>
	  <parameter id="-sla-pbs1-computer_guid">COMP-XXXX</parameter>
	  <parameter id="-sla-runner1-computer_guid">COMP-XXXX</parameter>
	  <parameter id="-sla-runner0-computer_guid">COMP-XXXX</parameter>
	</instance>

If you want to check by yourself that the pull-backup instances do their job, you can change directory to the slappart of runner0, and run ./bin/exporter (after creating your account, using the given backend_url or url): it would fill ./srv/backup/runner with data. If you then go to an import instance (runner1 or runner2) on the port 50005, you should be able to sign in the runner.


Tips :
~~~~~

To find in which partition the instance has been deployed, you can open the page of this specific instance, and look for "slappartXX" on the page.

Tests :
-------

For the tests, please refer to the README in slapos.toolbox.git/slapos/resiliencytest

Request custom frontend :
-------------------------

While deploying a server in your instance, you may need to ask for a custom ipv4 frontend.

The way to do that is to send a new parameter to your runner instance, using the parameters XML. The name of it is "custom-frontend-backend-url".

To create the frontend, you now have to wait the slaprunner to be processed.

If you deploy a server, which uses basic auth, you also have to declare the parameter "custom-frontend-basic-auth" as true, or your slaprunner instance won't show as correctly instanciated.

Example :
	<?xml version='1.0' encoding='utf-8'?>
	<instance>
	<parameter id="custom-frontend-backend-url">http(s)://[THE_IPV6_ADDRESS]:PORT</parameter>
	<parameter id="custom-frontend-basic-auth">true</parameter>
	</instance>
