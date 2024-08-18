# sish

An open source serveo/ngrok alternative.

[Read the docs.](https://docs.ssi.sh)

## dev

Clone the `sish` repo:

```bash
git clone git@github.com:antoniomika/sish.git
cd sish
```

Add your SSH public key:

```bash
cp ~/.ssh/id_ed25519.pub ./deploy/pubkeys
```

Run the binary:

```bash
go run main.go --http-address localhost:3000 --domain localhost
```

We have an alias `make dev` for running the binary.

SSH to your host to communicate with sish:

```bash
ssh -p 2222 -R firstt:80:httpbin.org:80 localhost
```
> The `testing.ssi.sh` DNS record points to `localhost` so anyone can use it for
> development

docker run -itd --name sish \
  -v ~/sish/ssl:/ssl \
  -v ~/sish/keys:/keys \
  -v ~/sish/pubkeys:/pubkeys \
  -p 2222:2222 \
  -p 80:80 \
  -p 443:443 \
  sha256:370097b1bb68b6e95aea5e29f1de05e62a3024a9cdb1518af6a24d60593f3e60 \
  --ssh-address=:2222 \
  --http-address=:80 \
  --https-address=:443 \
  --authentication=true \
  --authentication-password=password \
  --https-certificate-directory=/ssl \
  --authentication-keys-directory=/pubkeys \
  --private-keys-directory=/keys \
  --bind-random-ports=false \
  --admin-console=true \
  --log-to-client=true \
  --admin-console-token=admin \
  --domain=localhost


sshpass -p 'password' ssh -p 2222 -R firstt:80:httpbin.org:80 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 localhost

sshpass -p 'password' ssh -p 2222 -R secondtest:80:httpbin.org:80 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 localhost

sshpass -p 'password' ssh -p 2222 -R thirdtest:80:localhost:3754 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 localhost

sshpass -p 'password' ssh -p 2222 -R syss:80:httpbin.org:80 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 localhost

sshpass -p 'password' ssh -p 2222 -R syss:80:localhost:3754 -o StrictHostKeyChecking=no -o ServerAliveInterval=30 localhost