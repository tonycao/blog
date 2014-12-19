---
layout: post
title:  "Verify MD5 checksums on a MAC"
date:   2014-12-19 18:43:33
categories: Technical
---

If you want to verify an MD5 checksum for a file on a mac, you can just do:

	1. open terminal, type: md5 [filename] or openssl md5 [filename]

If you want to use md5sum like in Linux, you need install md5sha1sum first:

	1. sudo port install md5sha1sum
	2. type: md5sum [filename]