This is a modification to the portable version of
[acme-client](https://kristaps.bsd.lv/acme-client/) to eliminate the need to
run as root.

- No chroot(2).
- No privilege dropping by changing user.
- To compensate for losing chroot(2), we try to avoid path traversal in the one
  case where we write to a user-supplied path, the internally-handled http-01
  challenge, by actually validating that the token is base64-url characters.
- And if we're going to check that, we might as well also enforce the 128-bit
  entropy requirement ... it should never make a difference anyway unless the
  server is broken.

I have not necessarily thought through the security implications of these
changes, nor how they interact with the rest of the code's assumptions. It is
extremely plausible that using this code exposes gaping security holes that
will expose your private keys. Probably only to the ACME server, but who knows?

Caveat emptor.

You might also want to see [the original readme file](README.md).
