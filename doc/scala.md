# Scala

- Jdk
- Sbt (console :load
- IDE (intellij)

Val vs var (mutabile vs immutabile), definizioni, Espressioni no istruzioni(side effect)

## Object Oriented Proogramming

Classi

campi (parametri privati di default),
eq(reference) vs ==(valori)
metodi (prefix/infix…)
ereditarietà, package

Objects(singleton) x factory,utils, esempio App
- companion

Case class
- no new, toSting e hash, parametri val fileds, copy, pattern matching, (non possono ereditare)

Traits, ereditarietà e gerarchia dei tipi
(in guida spiegato dopo in parte funzionale ma anticipiamo)

stringhe

** Worksheet di esempio

## Test 

scalaTest (https://www.scalatest.org/)

test folder
diversi stili 
- FunSuite (xUnit - test/assert)
- FlatSpec (BDD - A should .. in)
- FunSpec (RSpec BDD - describe ..)
- WordSpec (specs or specs2 - A when ..)
- FreeSpec
- PropSpec
- FeatureSpec (acceptance test - feature/scenario )
- RefSpec

Matchers e tag(ignore)

## Collections

gerarchia e metodi utili
Seq, Vector, List, Tuple, Map
mutable vs immutable

** Worksheet di esempio

## Functional Programming

ADT (con trait e case class)

Higher order
map, filter, flatMap, fold,
for <- .. yield ..
per liste ma anche per Option ;-)

curry, uncurry

pattern matching
match .. case .. => ..

** Worksheet di esempio

## Altro

Failure (Trym Either), 
Future
Implicit (Type classes)
