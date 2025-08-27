# Challenge

> csawctf{$$king$_confusion$$$}

## Description

This challenge involves an algorithm confusion attack with an old vulnerable version of PyJWT. Tokens are signed using EdDSA (a public key algorithm). However, when the tokens are being decoded, jwt.algorithms.get_default_algorithms() is used. This allows an attacker to sign a forged token with the public key and change the algorithm to a symmetric algorithm (like HS256). The public key in this challenge can be leaked via an SSTI vulnerability on one of the pages.

## Tools

These will be needed to help install/solve challenge:

- Docker
- Python

## Installation

./install.sh

