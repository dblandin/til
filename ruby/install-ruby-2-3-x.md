# Install Ruby 2.3.x

Gravity, one the projects I contribute to at Artsy, is currently pinned to Ruby
`2.3.7`.

I use [ruby-install][] to install different versions of Ruby and [chruby][] to
switch between them.

The installation for Ruby `2.3.7` failed on my system due to what seemed to be a
missing version of OpenSSL. At the time, I had OpenSSL `v1.1.1a` installed.

Here's what the installation output looked like on my system:

<details>
 <summary>Expand output</summary>


```bash
$ ruby-install ruby 2.3.7
# --- SNIP ---
make[2]: Entering directory '/home/devon/src/ruby-2.3.7/ext/openssl'
compiling openssl_missing.c
In file included from openssl_missing.c:21:
openssl_missing.h:78:35: error: macro "EVP_MD_CTX_create" passed 1 arguments, but takes just 0
 EVP_MD_CTX *EVP_MD_CTX_create(void);
                                   ^
In file included from /usr/include/openssl/x509.h:18,
                 from /usr/include/openssl/x509_vfy.h:17,
                 from openssl_missing.c:15:
openssl_missing.h:82:6: error: expected declaration specifiers or ‘...’ before ‘(’ token
 void EVP_MD_CTX_init(EVP_MD_CTX *ctx);
      ^~~~~~~~~~~~~~~
openssl_missing.h:90:6: error: expected declaration specifiers or ‘...’ before ‘(’ token
 void EVP_MD_CTX_destroy(EVP_MD_CTX *ctx);
      ^~~~~~~~~~~~~~~~~~
openssl_missing.c:39:23: error: macro "EVP_MD_CTX_create" passed 1 arguments, but takes just 0
 EVP_MD_CTX_create(void)
                       ^
openssl_missing.c:40:1: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘{’ token
 {
 ^
openssl_missing.c: In function ‘EVP_MD_CTX_cleanup’:
openssl_missing.c:55:27: error: invalid application of ‘sizeof’ to incomplete type ‘EVP_MD_CTX’ {aka ‘struct evp_md_ctx_st’}
     memset(ctx, 0, sizeof(EVP_MD_CTX));
                           ^~~~~~~~~~
In file included from /usr/include/openssl/x509.h:18,
                 from /usr/include/openssl/x509_vfy.h:17,
                 from openssl_missing.c:15:
openssl_missing.c: At top level:
openssl_missing.c:63:1: error: expected declaration specifiers or ‘...’ before ‘(’ token
 EVP_MD_CTX_destroy(EVP_MD_CTX *ctx)
 ^~~~~~~~~~~~~~~~~~
openssl_missing.c:72:1: error: expected declaration specifiers or ‘...’ before ‘(’ token
 EVP_MD_CTX_init(EVP_MD_CTX *ctx)
 ^~~~~~~~~~~~~~~
openssl_missing.c: In function ‘HMAC_CTX_init’:
openssl_missing.c:82:25: error: dereferencing pointer to incomplete type ‘HMAC_CTX’ {aka ‘struct hmac_ctx_st’}
     EVP_MD_CTX_init(&ctx->i_ctx);
                         ^~
openssl_missing.c: In function ‘HMAC_CTX_cleanup’:
openssl_missing.c:95:27: error: invalid application of ‘sizeof’ to incomplete type ‘HMAC_CTX’ {aka ‘struct hmac_ctx_st’}
     memset(ctx, 0, sizeof(HMAC_CTX));
                           ^~~~~~~~
make[2]: *** [Makefile:302: openssl_missing.o] Error 1
make[2]: Leaving directory '/home/devon/src/ruby-2.3.7/ext/openssl'
make[1]: *** [exts.mk:212: ext/openssl/all] Error 2
make[1]: Leaving directory '/home/devon/src/ruby-2.3.7'
make: *** [uncommon.mk:203: build-ext] Error 2
!!! Compiling ruby 2.3.7 failed!
```

</details>
<br/>

After some [digging](https://github.com/rvm/rvm/issues/3862)
[around](https://github.com/rvm/rvm/issues/3862), I found my way to the [Rbenv
page](https://wiki.archlinux.org/index.php/Rbenv#Ruby_2.3.x) in the Arch Linux
Wiki (:tada:) which has a useful troubleshooting section specifically for
installations of Ruby `2.3.x`.

It linked to [this Stack Overflow question][so-question] and suggested the
following:

```bash
# install dependencies
pacman -S --needed base-devel libffi libyaml openssl zlib

# install OpenSSL 1.0
pacman -S openssl-1.0

# Install Ruby 2.3.x linking to OpenSSL 1.0
PKG_CONFIG_PATH=/usr/lib/openssl-1.0/pkgconfig ruby-install ruby 2.3.7
```

I'm using Arch Linux so this worked perfectly for me.

[chruby]: https://github.com/postmodern/chruby
[ruby-install]: https://github.com/postmodern/ruby-install
[so-question]: https://stackoverflow.com/questions/44116005/openssl-error-installing-ruby-2-1-x-and-2-3-x-on-archlinux-with-ruby-install-rub
