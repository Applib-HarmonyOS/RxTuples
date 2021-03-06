[![Build](https://github.com/applibgroup/RxTuples/actions/workflows/main.yml/badge.svg)](https://github.com/applibgroup/RxTuples/actions/workflows/main.yml)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=applibgroup_RxTuples&metric=alert_status)](https://sonarcloud.io/dashboard?id=applibgroup_RxTuples)
# RxTuples
RxTuples is a library to smooth RxJava usage by adding simple Tuple creation functions.

# Source
This library has been inspired by [pakoito\\RxTuples](https://github.com/pakoito/RxTuples) version 1.0, released on December 8, 2015.

## Features
Quite often when using RxJava you find the need to forward a value alongside the result of an operation, combine several values, or simply adding an external value to the current internal state of the chain. For this you either create ad-hoc types that may only be used locally, which is inefficient.

Other languages have the concept of a Tuple built into them, which is an in-place list of values. Lots of Java libraries implement their own concept of Tuple, being a Pair, a Point, or VecX types. This library uses [javatuples](http://www.javatuples.org/) in an attempt to unify them. Javatuples are all "typesafe, immutable, iterable, serializable, comparable" classes  ranging from 1 to 10 elements. 

This library has functions to combine multiple Observable objects into appropriate Pair, Triplet, Quartet, Quintet, Sextet, Septet, or Octet.

## Dependency

1. For using rxtuples module in sample app, include the source code and add the below dependencies in entry/build.gradle to generate hap/support.har.
```
 implementation project(path: ':rxtuples')
 testImplementation 'junit:junit:4.13'
```
2. For using rxtuples module in separate application using har file, add the har file in the entry/libs folder and add the dependencies in entry/build.gradle file.
```
 implementation fileTree(dir: 'libs', include: ['*.har'])
 implementation 'org.javatuples:javatuples:1.2'
 implementation 'io.reactivex:rxjava:1.3.8'
 testImplementation 'junit:junit:4.13'
```
3. For using rxtuples module from a remote repository in separate application, add the below dependencies in entry/build.gradle file.
```
dependencies {
		implementation 'dev.applibgroup:rxtuples:1.0.1'
        	testCompile 'junit:junit:4.12'
	}
```

## Usage
RxTuples come as lazily evaluated FuncN and its main use case is alongside the combineLatest, withLatestFrom, zip, and zipWith operators.

Zip a list element into a pair with their position:

    Observable.zip(Observable.from(myStringList), Observable.range(0, myStringList.size()), 
                   RxTuples.<String, Integer>toPair());

Merge the value of several hot observables:

    Observable.combineLatest(networkSubject(), bluetoothSubject(), compassSubject(), 
                             RxTuples.<NetworkStatus, BluetoothState, CompassPosition>toTriplet());

Get the previous element from a sequence alongside the current one:

    Observable.zip(compassSubject(), compassSubject().skip(1), 
                   RxTuples.<CompassPosition, CompassPosition>toPair());

or more complicated cases

    Observable.just(Quintet.with(1, 2, 3, 4, 5))
              .zipWith(
                    Observable.just(Triplet.with(6, 7, 8)),
                    RxTuples.<Integer, Integer, Integer, Integer, Integer, Integer, Integer, Integer> toOctetFromQuintet());


License
--------
Copyright (c) pakoito 2015

The Apache Software License, Version 2.0

See LICENSE.md
