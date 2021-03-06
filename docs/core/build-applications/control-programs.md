# Control Programs

## Introduction

A control program is the mechanism that secures asset units on a blockchain. When you first issue units of an asset, you issue them into a control program. When you spend units of an asset, you spend them from an existing control program to a new control program. When you retire units of an asset, you spend them from an existing control program to a special retirement control program that can never be spent.

Each output in a transaction contains a single control program. Each control program consists of set of predicates that must be satisfied in order to spend the output (i.e. use the output as an input to a new transaction).

## Overview

This guide will walk you through the basic types of control programs available in Chain Core:

* [Account control programs](#account-control-programs)
* [Retirement control programs](#retirement-control-programs)
* [Custom control programs](#custom-control-programs)

### Sample Code

All code samples in this guide are extracted from a single Java file.

<a href="../examples/java/ControlPrograms.java" class="downloadBtn btn success" target="\_blank">View Sample Code</a>

## Account control programs

The most basic type of control program is an account control program, which defines a set of keys and a quorum of signatures required to spend asset units. When you create an account, you provide a set of root keys and a quorum. Then each time you deposit assets into an account, Chain Core derives a new set of child public keys from the account root keys and creates a unique, one-time-use account control program requiring the quorum of signatures you specified.

Although all control programs in one account are controlled by keys derived from the same root keys, it is impossible for other participants on the blockchain to recognize any relationship between them. This technique (known as hierarchical deterministic key derivation) ensures that only the participant on the blockchain with whom you transact will know that a specific control program is yours. To everyone else, the creator of the control program will be unknown. For more information about key derivation, see the [Chain key derivation specification](../../protocol/specifications/chainkd.md).

### Example

If Alice wishes to be paid gold by an external party (Bob), she first creates a new control program in her account:

$code ../examples/java/ControlPrograms.java create-control-program

She then delivers the control program to Bob, who provides it to the transaction builder:

$code ../examples/java/ControlPrograms.java build-transaction

## Retirement control programs

A retirement control program is a very simple control program with a single predicate: `FAIL`. This ensures that asset units sent to this type of control can never be spent, and are thus removed from circulation on the blockchain.

### Example

To retire units of gold from Alice's account, we use the `SpendFromAccount` and `Retire` actions on the `Transaction.QueryBuilder`, which prompts Chain Core to create the retirement control program and spent to it from Alice's account.

$code ../examples/java/ControlPrograms.java retire

## Custom control programs

The [Chain Virtual Machine](../../protocol/specifications/vm1.md) supports custom control programs. We are currently developing a [high level language](../../protocol/papers/blockchain-programs.md#ivy) that will enable developers to write custom control programs in Chain Core. Additionally, we work directly with our enterprise customers to design, audit, and implement custom control programs for production deployment. For more information, visit the [Enterprise page](https://chain.com/enterprise).
