---
layout: post
title: "node coap and mbedtls"
subtitle:   "integrate"
date:       2016-09-22 14:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

# mbedtls read calls
~~~bash
/*
 * Receive application data decrypted from the SSL layer
 */
int mbedtls_ssl_read( mbedtls_ssl_context *ssl, unsigned char *buf, size_t len )

>>>

/*
 * Read a record.
 *
 * Silently ignore non-fatal alert (and for DTLS, invalid records as well,
 * RFC 6347 4.1.2.7) and continue reading until a valid record is found.
 *
 */
int mbedtls_ssl_read_record( mbedtls_ssl_context *ssl )

>>>

/*
 * If applicable, decrypt (and decompress) record content
 */
static int ssl_prepare_record_content( mbedtls_ssl_context *ssl )

>>>

static int ssl_decrypt_buf( mbedtls_ssl_context *ssl )

~~~
