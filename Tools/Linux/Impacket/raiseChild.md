This automates escalating from child to parent domain when having a Golden Ticket. We need to specify the target DC and credentials for an administrative user in the child domain; the script will do the rest.

# Useful Flags
- `target-exec`: authenticates to the parent domain's DC via [[PsExec]]