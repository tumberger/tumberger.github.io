---
title: "Simple Intro "
date: 2023-01-24T10:38:40+01:00
draft: true
tags: [Notes, MPC]
---

## Security Assumptions

### Adversarial Power

#### Semi-Honest Adversaries

#### Malicious Adversaries

### Corruption Strategy

#### Static Corruption

#### Adaptive Corruption

#### Proactive Corruption

### Arithmetic and Boolean Circuits

Arithmetic Circuits are turing complete, hence any function can be represented as an arithemtic circuit.

## Honest-Majority MPC from Secret-Sharing Schemes

<!-- Explain Shamir Secret Sharing Scheme -->
To explain how Honest-Majority MPC from Secret-Sharing Schemes works, we will first describe Shamirs' secret sharing algorithm in the following.
In Shamir's secret sharing scheme, a dealer wants to share a secret among $$n$$ parties, assuming that $$t < n$$ of them cannot learn anything about the secret and $$t+1$$ parties can reconstruct the secret.
Shamir's secret sharing scheme leverages the fact that any $$t+1$$ points in a two-dimensional plane can be interpolated by a polynomial of maximal degree $$t$$, e.g., through Lagrange interpolation.

In order to share a secret $$s$$, the dealer chooses a polynomial $$p(x)$$ at random such that $$p(0) = s$$.
Successively, each party $$i = 1, ..., n$$ receives a share $$y_i = p(i)$$. As Shamir's secret sharing scheme operates over a finite field of order $$p$$, $$p>n$$ needs to hold such that no two sharings distributed to the $$n$$ parties are the same. As the degree of the polynomial is $$t$$, $$t+1$$ parties can reconstruct the polynomial through interpolation and compute the secret by evaluating $$p(0) = s$$.

To compute any function represented as an arithemtic circuit in a distributed setting, a general MPC protocol based on secret sharing operates in three phases - *(i)* Sharing the input, *(ii)* Evaluating the circuit and *(iii)* Reconstructing the output.
As each party holds a certain number of wires that serve as input to the arithmetic circuit, the party holding the bitwise input to a wire acts as a dealer in the secret sharing protocol. Following this initial sharing of the input in a pre-processing phase, the circuit evaluation proceeds by evaluating the circuit gate by gate.
For each gate where the parties hold $$t+1$$ sharings of each of the two input wires, the resulting output wire value will be a $$t+1$$ out of $$n$$ sharing of the output.
Computing an addition gate can be done without further communication between the parties, as the output wire is a degree $$t$$ polynomial, such that $$c(0) = a(0) + b(0)$$ ($$c(x)$$ is the output polynomial, $$a(x)$$ and $$b(x)$$ are the both input polynomials to the addition gate).
For multiplication gates, however, the output polynomial $$c(x) = a(x) * b(x)$$ is of degree $$2t$$. Reducing the degree of a $$2t$$ polynomial to $$t$$ without changing $$c(0)$$ requires for additional communication between the MPC participants.

Note that the protcol above is only secure for less than $$\frac{n}{2}$$ adversarial parties, as the secret sharing scheme demands for $$n=2t+1$$ participants for a polynomial of degree $$t$$.

## Yao's Garbled Circuits

<!-- 
Yao's Garbled Circuits are based on OT. OT has been shown to be theoretically equivalent to MPC.
Given OT, one can build MPC without any additional assumptions, and the other way around.
Concept OT: Sender holds two secrets, receiver holds a choice. The receiver will receive one of the two, where the sender does not know which one was chosen and the receiver only learns one of the secrets.
* -->

A Garbling Scheme is defined as follows:

- Garble
- Encode
- Eval
- Decode

Overall, the scheme proposed here is fairly similar to the one proposed in DECO.
The authors first (i) apply the three party handshake to obtain shares of the client secret key between the prover and the verifier, (ii) Calculate in 2PC the key derivation and master secret, (iii) optimize the operation in 

| :memo:        | A large part of the notes in this post are based on the [Introduction to Secure Multiparty Computation](https://eprint.iacr.org/2020/300.pdf) by Yehuda Lindell.       |
|---------------|:------------------------|