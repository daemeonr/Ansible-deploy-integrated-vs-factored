Problem statement:
  - Client is a SAAS/PAAS provider of mobile banking solutions
  - Client has financial institution clients that will not allow agent
    based automation (Puppet, Chef, SaltStack) to be used to deploy
    SAAS/PAAS application installation and maintenance into their data centers
  - Client also has other/smaller financial institution cliets that do allow
    automated deployment
  - Client uses versions of various packages that are more current than the
    OS platform version (RHEL 6.6/6.7) such as Tomcat and Apache httpd
  - Internally client uses puppet to configure servers, then executs a
    complex series of manual scp/sftp steps and manual rpm installs and
    did NOT want to puppetize the application SAAS deployment, due to the
    extremely good and sophisticated Ansible toolset used.
  - Existing Ansible playbook automation (CB/CI/CD) had issues around an 
    automated (highly refactored) playbook component that deploys rpms 
    onto target systems then installs same (rpm, not yum)
  - Client had one extremely good Ansible developer who had been badly
    injured in an accident, and no other resources with the ability or
    bandwidth to come up to speed on existing Ansible toolset
  - Client had some very opinionated Chef engineers who wanted to force
    (no kidding) the major instituions to authorize agent based deploys

Recommendation:
  - build out a simple playbook that will do an initial deployment of RPM's
    which could then be integrated
  - Client asked that I build out a separate playbook to handle initial
    installs.
  - I recommended building & adding a local yum repo with required RPM's
  - I recommended a SINGLE playbook so that the less skilled engineers
    could easily follow the process.

Issues:
  - When the integrated playbook was build (see the integrated-playbook
    subdirectory), the Puppet engineers were annoyed that it was 
    too simple. I was "told" to refactor it into a modularized
    playbook.
  - When I completed the results in 3 days I was told that I was not
    sufficiently knowledgeable (by the engineers who did not like
    Ansible, and wanted to force Puppet) to support the effort.
  - Because my work was rejected, I believe it is OK to release this
    onto Github. Note that while the client makes extensive use of
    open source and github tools, they have absolutely zero contributions
    back.

