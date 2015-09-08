Problem statement:
  - Client is a SAAS/PAAS provider of mobile banking solutions
  - Client has financial institution clients that will not allow agent
    based automation (Puppet, Chef, SaltStack) to be used to deploy
    SAAS/PAAS application installation and maintenance into their data centers
  - Client also has other/smaller financial institution clients that do allow
    automated deployment
  - Client uses versions of various packages that are more current than the
    OS platform version (RHEL 6.6/6.7) such as Tomcat and Apache httpd
  - Internally client uses puppet to configure servers, then executes a
    complex series of manual scp/sftp steps and manual rpm installs and
    did NOT want to puppetize the application SAAS deployment, due to the
    extremely good and sophisticated Ansible toolset used.
  - Existing Ansible playbook automation (CB/CI/CD) had issues around an 
    automated (highly refactored) playbook component that deploys rpms 
    onto target systems then installs same (rpm, not yum)
  - Client had some very opinionated Chef engineers who wanted to force
    the major institutions to authorize puppet agent based deploys
    (or do agentless, with all the hiera and replicated content issues on
    every target or require NFS access or ...)

Recommendation:
  - Build out a simple playbook that will do an initial deployment of RPM's
    which could then be integrated back into the deployment engineer dvd's
  - Client asked that I build out a separate playbook to handle initial
    installs.
  - Build & deploy a local yum repo with required RPM's
  - A SINGLE playbook so that the less skilled engineers could easily 
    follow the process.

Issues:
  - When the simple, integrated playbook was build (see the 
    integrated-playbook subdirectory), the Puppet/anti-Ansible engineers 
    were annoyed that it was too simple (and did not take me long enough 
    to implement, I suspect?). I was "told" to refactor it into a modularized 
    playbook.
  - When I completed the results in 3 days I was told that I was not
    sufficiently knowledgeable (by the engineers who did not like
    Ansible, and wanted to force Puppet) to support the effort.
  - Because my work was rejected, I believe it is OK to release this
    onto Github. 
  - Note that while the client makes extensive use of open source and 
    github tools, they have absolutely zero contributions back. This
    includes the best tool chain for deploying from a single engineering
    build to highly variable distinct SAAS or PAAS landscapes or VPC's

HowTo:
  - There are two parallel playbooks in ths project:
    - One is ./Playbook-Modularized 
    - the other is ./Playbook-Integrated. 
    In discussions below, if there is a common directory (e.g. 
    Playbook-Modularized/files/RPMS and Playbook-Modularized/files/RPMS) 
    I will just refer to the subtree.  E.g. files/RPMS. Where there is a 
    difference, I will clarify.
    I WOULD APPRECIATE YOUR FEEDBACK!

  - Add desired packages to files/TOMCAT6PKGS, then add related rpm's (if
    not using the default distro) to files/RPMS (these will be copied over
    to the target system's local repo). Note that we do not delete this 
    repo (if I recall) after a successful install at client's request.
  - put the RPM's in ./files/RPMS/
  - update the various component versions, add additional applications, etc.
    follow the same directory tree naming convention. For the modularized 
    version, each application has its own subtree.
  - update your inventory host file (DaemeonEnv in this example)
  - execute the pulled examples as follows:
    - (cd Playbook-Modular ;  ansible-playbook -i DaemeonEnv checkinstall.yml)
    - (or) (cd Playbook-Integrated ;  ansible-playbook -i DaemeonEnv checkinstall.yml)
 
