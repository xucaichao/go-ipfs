# Contribute

go-ipfs is MIT licensed open source software. We welcome contributions big and
small! Take a look at the [community contributing notes](https://github.com/ipfs/community/blob/master/contributing.md). Please make sure to check the [issues](https://github.com/ipfs/go-ipfs/issues). Search the closed ones
before reporting things, and help us with the open ones.

## Go Guidelines:

Please look and conform to our [Go Contribution Guidelines](https://github.com/ipfs/community/blob/master/go-contribution-guidelines.md).

## General Guidelines:

- See the [dev pseudo-roadmap](dev.md).
- Please adhere to the protocol described in [the main ipfs repo](https://github.com/ipfs/ipfs), [paper](http://static.benet.ai/t/ipfs.pdf), and [specs](https://github.com/ipfs/specs) (WIP).
- Please make branches and pull-request, even if working on the main repository.
- Ask questions or talk about things in [Issues](https://github.com/ipfs/go-ipfs/issues) or #ipfs on freenode.
- Ensure you are able to contribute (no legal issues please-- we'll probably setup a CLA).
- Have fun!

## Repository specific guidelines:

### Commit messages

Commit messages must start with a short subject line, followed by an optional, 
more detailed explanatory text which is separated from the summary by an empty line. 
We use [GitCop](https://gitcop.com) to check that commit messages are
properly written. It checks the following:

* The first line of a commit message, called the subject line should
  not be more than 80 characters long.

* The commit message should end with the following trailers:

  ```
  License: MIT
  Signed-off-by: User Name <email@address>
  ```

  where "User Name" is the author's real (legal) name and
  email@address is one of the author's valid email addresses.

  These trailers mean that the author agrees with the 
  [developer certificate of origin](docs/developer-certificate-of-origin)
  and with licensing the work under the [MIT license](docs/LICENSE).

  To help you automatically add these trailers, you can run the
  [setup_commit_msg_hook.sh](https://github.com/ipfs/community/blob/master/dev/tools/hooks/setup_commit_msg_hook.sh)
  script which will setup a Git commit-msg hook that will add the above
  trailers to all the commit messages you write.

See the [documentation about amending commits](docs/amending-commits.md)
for explanation about how you can rework commit messages.
  
Some example commit messages:

```
parse_test: improve tests with stdin enabled arg

Now also check that we get the right arguments from
the parsing.

License: MIT
Signed-off-by: Christian Couder <chriscool@tuxfamily.org>
```

and

```
net/p2p + secio: parallelize crypto handshake

We had a very nasty problem: handshakes were serial so incoming
dials would wait for each other to finish handshaking. this was
particularly problematic when handshakes hung-- nodes would not
recover quickly. This led to gateways not bootstrapping peers
fast enough.

The approach taken here is to do what crypto/tls does:
defer the handshake until Read/Write[1]. There are a number of
reasons why this is _the right thing to do_:
- it delays handshaking until it is known to be necessary (doing io)
- it "accepts" before the handshake, getting the handshake out of the
  critical path entirely.
- it defers to the user's parallelization of conn handling. users
  must implement this in some way already so use that, instead of
  picking constants surely to be wrong (how many handshakes to run
  in parallel?)

[0] http://golang.org/src/crypto/tls/conn.go#L886

License: MIT
Signed-off-by: Juan Benet <juan@ipfs.io>
```