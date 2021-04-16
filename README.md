# Ansible Playbook to manage Server Certificates
This playbook will generate valid SSL certificates (using the ACME protocol) for any servers.  Servers can obtain multiple certificates, each certificate with multiple subjectAltNames if needed.

ACME authentication of domains is done against AWS Route53, so said servers do not need to be publicly accessible in any way, as long as they can communicate with AWS APIs.  You also don't have to worry about storing your AWS credentials on each machine, just your Ansible control machine.  This makes it harder to compromise your AWS credentials.  (Note: Credentials are passed and used on the remote machines, just not stored there.)


## TODO

### Refactor for multiple challenge plugins
No pre-processing of challenges and hard-coding for R53 like we currently do.  Instead, we should loop through all challenges, and just send the appropriate challenge data to the chosen plugin.  (See: https://github.com/felixfontein/acme-certificate/tree/main/tasks)

This may cause some performance issues we will need to investigate.  If we have multiple challenges, the R53 plugin may need to check available domain *each run*, which could incur a severe performance penalty on larger deployments.  Ideally we should condense the data structure before we send it off to the challenges, so challenges can do appropriate lookups only once.  This could be hard in raw Ansible, but might still be possible using the `combine()` or `map()` filters.

### Choose challenge plugins per-challenge?
Allow a user, via hosts.yml file, to select a plugin to use to verify an individual challenge.  This may be helpful, for example, if a user has most domains in R53, but one domain somewhere else.
