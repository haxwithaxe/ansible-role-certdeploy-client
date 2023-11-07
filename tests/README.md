# Testing
1. Edit the inventory as needed.
1. Run `ansible-playbook -i tests/inventory tests/test.yml`
1. Run the same command again to verify idempotence.
1. Run `ansible-playbook -i tests/inventory tests/verify.yml`

# Security

## DO NOT COPY AND PASTE THE TEST AS YOUR PLAYBOOK!
The private key is real but it's published publicly. Use your own key and load it into the variable from a file (encrypted or external).
