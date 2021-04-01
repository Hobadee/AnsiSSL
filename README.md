# Ansible Playbook to manage Server Certificates
This playbook will generate valid SSL certificates (using the ACME protocol) for any servers.  Servers can obtain multiple certificates, each certificate with multiple subjectAltNames if needed.

ACME authentication of domains is done against AWS Route53, so said servers do not need to be publicly accessible in any way, as long as they can communicate with AWS APIs.  You also don't have to worry about storing your AWS credentials on each machine, just your Ansible control machine.  This makes it harder to compromise your AWS credentials.  (Note: Credentials are passed and used on the remote machines, just not stored there.)
