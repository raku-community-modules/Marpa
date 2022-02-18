[![Actions Status](https://github.com/raku-community-modules/Marpa/workflows/test/badge.svg)](https://github.com/raku-community-modules/Marpa/actions)

NAME
====

Marpa - Raku interface to the libmarpa C library.

SYNOPSIS
========

```raku
use Marpa;
```

DESCRIPTION
===========

Marpa is a Raku interface to the libmarpa C library.

Marpa requires libmarpa to be present. I'd recommend installing from packages, or just look on the libmarpa website for install instructions.

AUTHOR
======

Jeffrey Goff, aka DrForr

Source can be located at: https://github.com/raku-community-modules/Marpa . Comments and Pull Requests are welcome.

COPYRIGHT AND LICENSE
=====================

Copyright 2015 - 2018 Jeffrey Goff

Copyright 2019 - 2022 Raku Community

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

### sub marpa_check_version

```raku
sub marpa_check_version(
    Int $required_major,
    Int $required_minor,
    Int $required_micro
) returns Marpa-Error-Code
```

Checks that the Marpa library in use is compatible with the given version. Generally you will pass in the constants MARPA-MAJOR-VERSION, MARPA-MINOR-VERSION, MARPA-MICRO-VERSION as the three arguments to this function; that produces a check that the library in use is compatible with the version of Libmarpa the application or module was compiled against. Compatibility is defined by two things: first the version of the running library is newer than the version required_major.required_minor.required_micro. Second the running library must be binary compatible with the version required_major.required_minor.required_micro (same major version.) Success value: MARPA-ERR-NONE if the Marpa library is compatible with the requested version. Failure value: If the library is not compatible, one of the following is returned, indicating the nature of the mismatch: MARPA-ERR-MAJOR-VERSION-MISMATCH, MARPA-ERR-MINOR-VERSION-MISMATCH MARPA-ERR-MICRO-VERSION-MISMATCH

### sub marpa_version

```raku
sub marpa_version(
    NativeCall::Types::Pointer[Int] $version
) returns Marpa-Error-Code
```

Returns the version number in version, which must have room for three int's. Success value: A non-negative number. Failure value: −2.

### sub marpa_c_init

```raku
sub marpa_c_init(
    NativeCall::Types::Pointer[Marpa::Marpa-Config] $config
) returns Int
```

Initialize the config information to “safe” default values. Unspecified behavior will result if an initialized configuration is used to create a grammar. Success value: A non-negative value. Failure value: Always succeeds.

### sub marpa_c_error

```raku
sub marpa_c_error(
    NativeCall::Types::Pointer[Marpa::Marpa-Config] $config,
    NativeCall::Types::Pointer[Str] $p_error_string
) returns Marpa-Error-Code
```

Error codes are usually kept in the base grammar, which leaves marpa_g_new() no place to put its error code on failure. Objects of the Marpa-Config class provide such a place. p_error_string is reserved for use by the internals. Applications should set it to NULL. Success value: The error code in config. Failure value: Always succeeds.

### sub marpa_g_new

```raku
sub marpa_g_new(
    NativeCall::Types::Pointer[Marpa::Marpa-Config] $configuration
) returns Marpa::g
```

Creates a new grammar time object. The returned grammar object is not yet precomputed, and will have no symbols and rules. Its reference count will be 1. Unless the application calls marpa_c_error, Libmarpa will not reference the location pointed to by the configuration argument after marpa_g_new returns. The configuration argument may be NULL, but if it is, there will be no way to determine the error code on failure. Success value: the grammar object. Failure value: NULL, and the error code is set in configuration.

### sub marpa_g_force_valued

```raku
sub marpa_g_force_valued(
    Marpa::g $g
) returns Int
```

It is recommended that this call be made immediately after the grammar constructor. It turns off a deprecated feature. The marpa_g_force_valued forces all the symbols in a grammar to be “valued”. The opposite of a valued symbol is one about whose value you do not care. This distinction has been made in the past in hope of gaining efficiencies at evaluation time. Current thinking is that the gains do not repay the extra complexity. Success value: a non-negative integer. Failure value: a negative integer.

### sub marpa_g_ref

```raku
sub marpa_g_ref(
    Marpa::Marpa-Grammar $g
) returns Marpa::g
```

Increases the reference count by 1. Not needed by most applications. Success value: the grammar object it was called with. Failure value: NULL.

### sub marpa_g_unref

```raku
sub marpa_g_unref(
    Marpa::g $g
) returns Mu
```

Decreases the reference count by 1, destroying g once the reference count reaches zero.

### sub marpa_g_start_symbol

```raku
sub marpa_g_start_symbol(
    Marpa::g $g
) returns Marpa::Marpa-Symbol-ID
```

Returns current value of the start symbol of grammar g. The value is that specified in the marpa_g_start_symbol_set() call, if there has been one. Success value: The new start symbol. Failure value: -1 if there is no start symbol, otherwise -2.

### sub marpa_g_start_symbol_set

```raku
sub marpa_g_start_symbol_set(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Marpa::Marpa-Symbol-ID
```

Sets the start symbol of grammar g to symbol id. Success value: The ID of the new start symbol. Failure value: -1 if sym_id is well-formed but there is no such symbol, otherwise -2.

### sub marpa_g_highest_symbol_id

```raku
sub marpa_g_highest_symbol_id(
    Marpa::g $g
) returns Int
```

Success value: the numerically largest symbol ID of $g. Failure value: -2.

### sub marpa_g_symbol_is_accessible

```raku
sub marpa_g_symbol_is_accessible(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

A symbol is accessible if it can be reached from the start symbol. Success value: 1 if symbol sym_id is accessible, 0 if not. If sym_id is well-formed, but there is no such symbol, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: -2.

### sub marpa_g_symbol_is_nullable

```raku
sub marpa_g_symbol_is_nullable(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

A symbol is nullable if it sometimes produces the empty string. A nulling symbol is always a nullable symbol, but not all nullable symbols are nulling symbols. Success value: 1 if symbol sym_id is nullable, 0 if not. If sym_id is well-formed, but there is no such symbol, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: -2.

### sub marpa_g_symbol_is_nulling

```raku
sub marpa_g_symbol_is_nulling(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

A symbol is nulling if it always produces the empty string. Success value: 1 if symbol sym_id is nulling, 0 if not. If sym_id is well-formed, but there is no such symbol, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: -2.

### sub marpa_g_symbol_is_productive

```raku
sub marpa_g_symbol_is_productive(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

A symbol is productive if it can produce a string of terminals. All nullable symbols are considered productive. Success value: 1 if symbol sym_id is productive, 0 if not. If sym_id is well-formed, but there is no such symbol, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: -2.

### sub marpa_g_symbol_is_start

```raku
sub marpa_g_symbol_is_start(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

This return value of this call indicates whether sym_id is the start symbol. Success value: 1 if sym_id is the start symbol. 0 if sym_id is not the start symbol, either because the start symbol is different from sym_id, or because the start symbol has not been set yet. Failure value: -1 if sym_id is well-formed, but there is no such symbol. -2 on other failure.

### sub marpa_g_symbol_is_terminal_set

```raku
sub marpa_g_symbol_is_terminal_set(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $value
) returns Int
```

These methods, respectively, set and query the “terminal status” of a symbol. To be used as an input symbol in the marpa_r_alternative() method, a symbol must be a terminal. This function flags symbol sym_id as a terminal if value is 1, or flags it as a non-terminal if value is 0. Once set to a value with the marpa_g_symbol_is_terminal_set() method, the terminal status of a symbol is “locked” at that value. A subsequent call to marpa_g_symbol_is_terminal_set() that attempts to change the terminal status of sym_id to a value different from its current one will fail. The error code will be MARPA-ERR-TERMINAL-IS-LOCKED. By default, a symbol is a terminal if and only if it does not appear on the LHS of any rule. An attempt to flag a nulling symbol as a terminal will cause a failure, but this is not necessarily detected before precomputation. Success returns: When successful, these methods return 1 if symbol sym_id is a terminal symbol after the call, 0 otherwise. Failure returns: If sym_id is well-formed, but there is no such symbol, −1. On all other failures, −2. Other failures include when value is not 0 or 1; when the terminal status is locked and value is different from its current value; and, in the case of marpa_g_symbol_is_terminal_set(), when the grammar g is precomputed.

### sub marpa_g_symbol_new

```raku
sub marpa_g_symbol_new(
    Marpa::g $g
) returns Marpa::Marpa-Symbol-ID
```

Creates a new symbol. The symbol ID will be a non-negative integer. Success value: The ID of a new symbol. Failure value: −2.

### sub marpa_g_highest_rule_id

```raku
sub marpa_g_highest_rule_id(
    Marpa::g $g
) returns Int
```

Success value: The ID of the numerically largest rule ID of $g. Failure value: −2.

### sub marpa_g_rule_is_accessible

```raku
sub marpa_g_rule_is_accessible(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

A rule is accessible if it can be reached from the start symbol. A rule is accessible if and only if its LHS symbol is accessible. The start rule is always an accessible rule. Success value: 1 if rule rule_id is accessible, 0 if not. If rule_id is well-formed, but there is no such rule, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: −2.

### sub marpa_g_rule_is_nullable

```raku
sub marpa_g_rule_is_nullable(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $ruleid
) returns Int
```

A rule is nullable if it sometimes produces the empty string. A nulling rule is always a nullable rule, but not all nullable rules are nulling rules. Success value: 1 if rule ruleid is nullable, 0 if not. If rule_id is well-formed, but there is no such rule, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: −2.

### sub marpa_g_rule_is_nulling

```raku
sub marpa_g_rule_is_nulling(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $ruleid
) returns Int
```

A rule is nulling if it always produces the empty string. Success value: 1 if rule ruleid is nulling, 0 if not. If rule_id is well-formed, but there is no such rule, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: −2.

### sub marpa_g_rule_is_loop

```raku
sub marpa_g_rule_is_loop(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

A rule is a loop rule if it non-trivially produces the string of length one which consists only of its LHS symbol. Such a derivation takes the parse back to where it started, hence the term “loop”. “Non-trivially” means the zero-step derivation does not count — the derivation must have at least one step. The presence of a loop rule makes a grammar infinitely ambiguous, and applications will typically want to treat them as fatal errors. But nothing forces an application to do this, and Marpa will successfully parse and evaluate grammars with loop rules. Success value: 1 if rule rule_id is a loop rule, 0 if not. If rule_id is well-formed, but there is no such rule, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: −2.

### sub marpa_g_rule_is_productive

```raku
sub marpa_g_rule_is_productive(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

A rule is productive if it can produce a string of terminals. An rule is productive if and only if all the symbols on its RHS are productive. The empty string counts as a string of terminals, so that a nullable rule is always a productive rule. For that same reason, an empty rule is considered productive. Success value: 1 if rule rule_id is productive, 0 if not. If rule_id is well-formed, but there is no such rule, −1. If the grammar is not precomputed, or on other failure, −2. Failure value: −2.

### sub marpa_g_rule_length

```raku
sub marpa_g_rule_length(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

The length of a rule is the number of symbols on its RHS. Success value: The length of rule $rule_id. Failure value: −2.

### sub marpa_g_rule_lhs

```raku
sub marpa_g_rule_lhs(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Marpa::Marpa-Symbol-ID
```

Success value: The LHS symbol of rule rule_id. If rule_id is well-formed, but there is no such rule, −1. Failure value: −2.

### sub marpa_g_rule_new

```raku
sub marpa_g_rule_new(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $lhs_id,
    NativeCall::Types::Pointer[Marpa::Marpa-Symbol-ID] $rhs_ids,
    Int $length
) returns Marpa::Marpa-Rule-ID
```

Creates a new external BNF rule in grammar g. The ID of the new rule will be a non-negative integer, which will be unique to that rule. In addition to BNF rules, Marpa also allows sequence rules, which are created by another method: marpa_g_sequence_new(). Sequence rules and BNF rules are numbered in the same series, so that a BNF rule will never have the same rule ID as a sequence rule, and vice versa. The LHS symbol is lhs_id, and there are length symbols on the RHS. The RHS symbols are in an array pointed to by rhs_ids. Possible failures, with their error codes, include: MARPA-ERR-SEQUENCE-LHS-NOT-UNIQUE: The LHS symbol is the same as that of a sequence rule. MARPA-ERR-DUPLICATE-RULE: The new rule would duplicate another BNF rule. Another BNF rule is considered the duplicate of the new one, if its LHS symbol is the same as symbol lhs_id, if its length is the same as length, and if its RHS symbols match one for one those in the array of symbols rhs_ids. Success value: the ID of new external rule. Failure value: −2.

### sub marpa_g_rule_rhs

```raku
sub marpa_g_rule_rhs(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id,
    int $ix
) returns Marpa::Marpa-Symbol-ID
```

Returns the ID of the symbol in position ix in the RHS of rule rule_id. The RHS position, ix, is zero-based. Success value: The ID of the symbol in position ix on the rules RHS. If rule_id is well-formed, but there is no such rule, −1. If ix is greater than or equal to the length of the rule, or on other failure, −2. Failure value: −2.

### sub marpa_g_rule_is_proper_separation

```raku
sub marpa_g_rule_is_proper_separation(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

Success returns: marpa_g_rule_is_proper_separation() succeeds if and only if rule_id is valid. If rule rule_id is a sequence rule where the proper separation flag is set, returns 1. On other success, including the case where rule rule_id is not a sequence rule, returns 0. Failure returns: If rule_id is well-formed, but there is no such rule, returns −1. On other failure, −2.

### sub marpa_g_sequence_min

```raku
sub marpa_g_sequence_min(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

Returns the mininum length of a sequence rule. This accessor can be also be used to test whether or not a rule is a sequence rule. −1 is returned if and only if the rule is valid but not a sequence rule. Success value: If rule rule_id is a sequence rule, its minimum length. If rule rule_id is valid, but is not the rule ID of sequence rule, −1. If rule_id is well-formed, but there is no such rule, or on other failure, −2. Failure value: −2.

### sub marpa_g_sequence_new

```raku
sub marpa_g_sequence_new(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $lhs_id,
    Marpa::Marpa-Symbol-ID $rhs_id,
    Marpa::Marpa-Symbol-ID $separator_id,
    Int $min,
    Int $flags
) returns Marpa::Marpa-Rule-ID
```

Adds a new sequence rule to grammar g. The ID of the new sequence rule will be a non-negative integer, which is unique to that rule. Sequence rules and BNF rules are numbered in the same series, so that a BNF rule will never have the same rule ID as a sequence rule, and vice versa. The sequence is lhs_id, and the item to be repeated in the sequence is rhs_id. The sequence must be repeated at least min times, where min is 0 or 1. If separator_id is non-negative, it is a separator symbol. If flags & MARPA-PROPER-SEPARATION is non-zero, separation is “proper”, that is, a trailing separator is not allowed. The term proper is based on the idea that properly-speaking, separators should actually separate items. Some higher-level Marpa interfaces offer the ability to discard separators in the semantics, and in fact will do this by default. At the Libmarpa level, sequences always “keep separators”. It is up to the programmer to arrange to discard separators, if that is what is desired. The sequence RHS, or item, is restricted to a single symbol, and that symbol cannot be nullable. If separator_id is a symbol, it also cannot be a nullable symbol. Nullables on the RHS of sequences are restricted because they lead to highly ambiguous grammars. Grammars of this kind are allowed by Libmarpa, but they must be expressed using BNF rules, not sequence rules. This is for two reasons: First, sequence optimizations would not work in the presence of nullables. Second, since it is not completely clear what an application intends when it asks for a sequence of identical items, some of which are nullable, the user's intent can be more clearly expressed directly in BNF. The LHS symbol cannot be the LHS of any other rule, whether a BNF rule or a sequence rule. On an attempt to create an sequence rule with a duplicate LHS, marpa_g_sequence_new() fails, setting the error code to MARPA-ERR-SEQUENCE-LHS-NOT-UNIQUE. Sequence rules do not add to the classes of grammar parsed by Libmarpa — a sequence can always be written as BNF rules. When a rule is created with the marpa_g_sequence_new() method, Libmarpa will understand that it is a sequence, and will optimize accordingly. The speedup is often considerable. Success value: The ID of the external rule. Failure value: −2.

### sub marpa_g_sequence_separator

```raku
sub marpa_g_sequence_separator(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id
) returns Int
```

Success value: If rule rule_id is a sequence rule, its separator. If rule rule_id is a sequence rule, but there is no separator, −1. If rule_id is not a sequence rule, does not exist or is not well-formed; or on other failure, −2. Failure value: −2.

### sub marpa_g_symbol_is_counted

```raku
sub marpa_g_symbol_is_counted(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

A symbol is counted if it appears on the RHS of a sequence rule, or if it is used as the separator symbol of a sequence rule. Success return: Returns 1 if symbol sym_id is counted, 0 if not. Failure return: If sym_id is well-formed, but there is no such symbol, −1. If sym_id is not well-formed, and on other failure, −2.

### sub marpa_g_rule_rank_set

```raku
sub marpa_g_rule_rank_set(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id,
    Marpa::Marpa-Rank $rank
) returns Marpa::Marpa-Rank
```

These methods, respectively, set and query the rank of a rule rule_id. When rule_id is created, its rank initialized to the default rank of the grammar. Initially, the default rank of the grammar is 0. Methods to reset the grammar's default rank are currently in the experimental stage. Success value: Returns the rank after the call, and sets the error code to MARPA-ERR-NONE. On failure, returns −2, and sets the error code to an appropriate value, which will never be MARPA-ERR-NONE. Note that when the rank is −2, the error code is the only way to distinguish success from failure. The error code can be determined by using the marpa_g_error() call. Failure value: −2.

### sub marpa_g_rule_null_high_set

```raku
sub marpa_g_rule_null_high_set(
    Marpa::g $g,
    Marpa::Marpa-Rule-ID $rule_id,
    Int $flag
) returns Int
```

These methods, respectively, set and query the “null ranks high” setting of the rule rule_id. The “null ranks high” setting is either 0 or 1. When rule_id is created, its “null ranks high” setting is initialized to 0. The “null ranks high” setting affects the ranking of rules with properly nullable symbols on their right hand side. If a rule has properly nullable symbols on its RHS, each instance in which it appears in a parse will have a pattern of nulled and non-nulled symbols. Such a pattern is called a “null variant”. If the “null ranks high” setting is 0 (the default), nulled symbols rank low. If the “null ranks high” setting is 1, nulled symbols rank high. Ranking of a null variants is done from left-to-right. The marpa_g_rule_null_high_set() method will return failure after the grammar has been precomputed. If there is no other cause of failure, the marpa_g_rule_null_high() method succeeds on both precomputed and unprecomputed grammars. Success value: The value of the “null ranks high” flag after the call. Failure value: If rule_id is well-formed, but there is no such rule, −1. On all other failures, −2.

### sub marpa_g_completion_symbol_activate

```raku
sub marpa_g_completion_symbol_activate(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $reactivate
) returns Int
```

Allows the user to deactivate and reactivate symbol completion events in the grammar. When a recognizer is created, the activation status of each of its events is initialized to the activation status of that event in the base grammar. If reactivate is zero, the event is deactivated in the grammar. If reactivate is one, the event is activated in the grammar. Symbol completion events are active by default if the symbol was set up for completion events in the grammar. If a symbol was not set up for completion events in the grammar, symbol completion events are inactive by default and any attempt to change that is a fatal error. The activation status of a completion event in the grammar can only be changed if the symbol is marked as a completion event symbol in the grammar, and before the grammar is precomputed. However, if a symbol is marked as a completion event symbol in the recognizer, the completion event can be deactivated and reactivated in the recognizer. Success cases: On success, the method returns the value of reactivate. The method succeeds trivially if the symbol is already set as indicated by reactivate. Failure cases: If the active status of the completion event for sym_id cannot be set as indicated by reactivate, the method fails. On failure, −2 is returned.

### sub marpa_g_nulled_symbol_activate

```raku
sub marpa_g_nulled_symbol_activate(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $reactivate
) returns Int
```

Allows the user to deactivate and reactivate symbol nulled events in the grammar. When a recognizer is created, the activation status of each of its events is initialized to the activation status of that event in the base grammar. If reactivate is zero, the event is deactivated in the grammar. If reactivate is one, the event is activated in the grammar. Symbol nulled events are active by default if the symbol was set up for nulled events in the grammar. If a symbol was not set up for nulled events in the grammar, symbol nulled events are inactive by default and any attempt to change that is a fatal error. The activation status of a nulled event in the grammar can only be changed if the symbol is marked as a nulled event symbol in the grammar, and before the grammar is precomputed. However, if a symbol is marked as a nulled event symbol in the recognizer, the nulled event can be deactivated and reactivated in the recognizer. Success value: On success, the method returns the value of reactivate. The method succeeds trivially if the symbol is already set as indicated by reactivate. Failure value: If the active status of the nulled event for sym_id cannot be set as indicated by reactivate, the method fails. On failure, −2 is returned.

### sub marpa_g_prediction_symbol_activate

```raku
sub marpa_g_prediction_symbol_activate(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $reactivate
) returns Int
```

Allows the user to deactivate and reactivate symbol prediction events in the grammar. When a recognizer is created, the activation status of each of its events is initialized to the activation status of that event in the base grammar. If reactivate is zero, the event is deactivated in the grammar. If reactivate is one, the event is activated in the grammar. Symbol prediction events are active by default if the symbol was set up for prediction events in the grammar. If a symbol was not set up for prediction events in the grammar, symbol prediction events are inactive by default and any attempt to change that is a fatal error. The activation status of a prediction event in the grammar can only be changed if the symbol is marked as a prediction event symbol in the grammar, and before the grammar is precomputed. However, if a symbol is marked as a prediction event symbol in the recognizer, the prediction event can be deactivated and reactivated in the recognizer. Success cases: On success, the method returns the value of reactivate. The method succeeds trivially if the symbol is already set as indicated by reactivate. Failure cases: If the active status of the prediction event for sym_id cannot be set as indicated by reactivate, the method fails. On failure, −2 is returned.

### sub marpa_g_symbol_is_completion_event

```raku
sub marpa_g_symbol_is_completion_event(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

Libmarpa can be set up to generate an MARPA-EVENT-SYMBOL-COMPLETED event whenever the symbol is completed. A symbol is said to be completed when a non-nulling rule with that symbol on its LHS is completed. For completion events to occur, the symbol must be marked as a completion event symbol. The marpa_g_symbol_is_completion_event_set() function marks symbol sym_id as a completion event symbol if value is 1, and unmarks it it as a completion event symbol if value is 0. The marpa_g_symbol_is_completion_event() method returns the current value of the completion event marking for symbol sym_id. Marking a completion event sets its activation status to on. Unmarking a completion event sets its activation status to off. The completion event marking cannot be changed once the grammar is precomputed. If a completion event is marked, its activation status can be changed using the marpa_g_completion_symbol_activate() method. Note that, if a symbol is marked as a completion event symbol in the recognizer, its completion event can be deactivated and reactivated in the recognizer. Nulled rules and symbols will never cause completion events. Nullable symbols may be marked as completion event symbols, but this will have an effect only when the symbol is not nulled. Nulling symbols may be marked as completion event symbols, but no completion events will ever be generated for a nulling symbol. Note that this implies at no completion event will ever be generated at earleme 0, the start of parsing. Success value: On success, 1 if symbol sym_id is a completion event symbol after the call, 0 otherwise. Failure value: If sym_id is well-formed, but there is no such symbol, −1. If the grammar g is precomputed; or on other failure, −2.

### sub marpa_g_symbol_is_nulled_event

```raku
sub marpa_g_symbol_is_nulled_event(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

Libmarpa can set up to generate an MARPA-EVENT-SYMBOL-NULLED event whenever the symbol is nulled. A symbol is said to be nulled when a zero length instance of that symbol is recognized. For nulled events to occur, the symbol must be marked as a nulled event symbol. The marpa_g_symbol_is_nulled_event_set() function marks symbol sym_id as a nulled event symbol if value is 1, and unmarks it it as a nulled event symbol if value is 0. The marpa_g_symbol_is_nulled_event() method returns the current value of the nulled event marking for symbol sym_id. Marking a nulled event sets its activation status to on. Unmarking a nulled event sets its activation status to off. The nulled event marking cannot be changed once the grammar is precomputed. If a nulled event is marked, its activation status can be changed using the marpa_g_nulled_symbol_activate() method. Note that, if a symbol is marked as a nulled event symbol in the recognizer, its nulled event can be deactivated and reactivated in the recognizer. As a reminder, a symbol instance is a symbol at a specific location in the input, and with a specific length. Also, whenever a nulled symbol instance is recognized at a location, it is acceptable at that location, and vice versa. When a symbol instance is recognized at a location, it will generate a nulled event or a prediction event, but never both. A symbol instance of zero length, when recognized at a location, generates a nulled event at that location, and does not generate a completion event. A symbol instance of non-zero length, when acceptable at a location, generates a completion event at that location, and does not generate a nulled event. When a symbol instance is acceptable at a location, it will generate a nulled event or a prediction event, but never both. A symbol instance of zero length, when acceptable at a location, generates a nulled event at that location, and does not generate a prediction event. A symbol instance of non-zero length, when acceptable at a location, generates a prediction event at that location, and does not generate a nulled event. While it is not possible for a symbol instance to generate both a nulled event and a completion event at a location, it is quite possible that a symbol might generate both kinds of event at that location. This is because multiple instances of the same symbol may be recognized at a given location, and these instances will have different lengths. If one instance is recognized at a given location as zero length and a second, non-zero-length, instance is recognized at the same location, the first will generate only nulled events, while the second will generate only completion events. For similar reasons, while a symbol instance will never generate both a null event and a prediction event at a location, multiple instances of the same symbol may do so. Zero length derivations can be ambiguous. When a zero length symbol is recognized, all of its zero-length derivations are also considered to be recognized. Success: On success, 1 if symbol sym_id is a nulled event symbol after the call, 0 otherwise. Failures: If sym_id is well-formed, but there is no such symbol, −1. If the grammar g is precomputed; or on other failure, −2.

### sub marpa_g_symbol_is_prediction_event

```raku
sub marpa_g_symbol_is_prediction_event(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id
) returns Int
```

Libmarpa can be set up to generate a MARPA-EVENT-SYMBOL-PREDICTED event when a non-nulled symbol is predicted. A non-nulled symbol is said to be predicted when a instance of it is acceptable at the current earleme according to the grammar. Nulled symbols do not generate predictions. For predicted events to occur, the symbol must be marked as a predicted event symbol. The marpa_g_symbol_is_predicted_event_set() function marks symbol sym_id as a predicted event symbol if value is 1, and unmarks it it as a predicted event symbol if value is 0. The marpa_g_symbol_is_predicted_event() method returns the current value of the predicted event marking for symbol sym_id. Marking a prediction event sets its activation status to on. Unmarking a prediction event sets its activation status to off. The prediction event marking cannot be changed once the grammar is precomputed. If a prediction event is marked, its activation status can be changed using the marpa_g_prediction_symbol_activate() method. Note that, if a symbol is marked as a prediction event symbol in the recognizer, its prediction event can be deactivated and reactivated in the recognizer. Success: On success, 1 if symbol sym_id is a predicted event symbol after the call, 0 otherwise. Failures: If sym_id is well-formed, but there is no such symbol, −1. If the grammar g is precomputed; or on other failure, −2.

### sub marpa_g_precompute

```raku
sub marpa_g_precompute(
    Marpa::g $g
) returns Int
```

Precomputation is necessary for a recognizer to be generated from a grammar. On success, marpa_g_precompute returns a non-negative number to indicate that it precomputed the grammar without issues. On failure, marpa_g_precompute returns −2. Precomputation may return one or more events, which may be queried using the marpa_g_event() method. At this point events only occur when failure is reported, and events always report issues. But application writers should expect future versions to have events which are reported on success, as well as events which do not represent issues. A MARPA-EVENT-LOOP-RULES event occurs when there are infinite loop rules (cycles) in the grammar. The presence of one or more of these will cause failure to be reported, but will not prevent the grammar from being precomputed. Each MARPA-EVENT-COUNTED-NULLABLE event is a symbol which is a nullable on the right hand side of a sequence rule — a “counted” symbol. The presence of one or more of these will cause failure to be reported, and will prevent the grammar from being precomputed. So that the programmer can fix several at once, these failures are delayed until events are created for all of the counted nullables. Each MARPA-EVENT-NULLING-TERMINAL event is a nulling symbol which is also flagged as a terminal. Since terminals cannot be of zero length, this is a logical impossibility. The presence of one or more of these will cause failure to be reported, and will prevent the grammar from being precomputed. So that the programmer can fix several at once, the failure is delayed until events are created for all of the counted nullables. Precomputation involves freezing and then thoroughly checking the grammar. Among the reasons for precomputation to fail are the following: MARPA-ERR-NO-RULES: The grammar has no rules. MARPA-ERR-NO-START-SYMBOL: No start symbol was specified. MARPA-ERR-INVALID-START-SYMBOL: A start symbol ID was specified, but it is not the ID of a valid symbol. MARPA-ERR-START-NOT-LHS: The start symbol is not on the LHS of any rule. MARPA-ERR-UNPRODUCTIVE-START: The start symbol is not productive. MARPA-ERR-COUNTED-NULLABLE: A symbol on the RHS of a sequence rule is nullable. Libmarpa does not allow this. MARPA-ERR-NULLING-TERMINAL: A terminal is also a nulling symbol. Libmarpa does not allow this. More details of these can be found under the description of the appropriate code. See External error codes. marpa_g_precompute() is unusual in that it is possible to treat one of its failures as “advisory”, and to proceed with parsing. If marpa_g_precompute() fails with an error code of MARPA-ERR-GRAMMAR-HAS-CYCLE, parsing can proceed, just as it typically would for success. The grammar will have been precomputed, as calling the marpa_g_is_precomputed() method will confirm. Most applications, however, will want to simply treat failure with MARPA-ERR-GRAMMAR-HAS-CYCLE, as simply another failure, and fix the cycles before parsing. Cycles make a grammar infinitely ambiguous, and are considered useless in current practice. Cycles make processing the grammar less efficient, sometimes considerably so. Detection of cycles is returned as failure because that is by far the convenient thing to do for the vast majority of applications. Success value: A non-negative number. Failure value: −2.

### sub marpa_g_is_precomputed

```raku
sub marpa_g_is_precomputed(
    Marpa::g $g
) returns Int
```

Success value: 1 if grammar g is already precomputed, 0 otherwise. Failure value: −2.

### sub marpa_g_has_cycle

```raku
sub marpa_g_has_cycle(
    Marpa::g $g
) returns Int
```

This function allows the application to determine if grammar g has a cycle. As mentioned, most applications will want to treat these as fatal errors. To determine which rules are in the cycle, marpa_g_rule_is_loop() can be used. Success value: 1 if the grammar has a cycle, 0 otherwise. Failure value: −2.

### sub marpa_r_new

```raku
sub marpa_r_new(
    Marpa::g $g
) returns Marpa::Marpa-Recognizer
```

Creates a new recognizer. The reference count of the recognizer will be 1. The reference count of g, the base grammar, will be incremented by one. Success value: The newly created recognizer. If g is not precomputed, or on other failure, NULL. Failure value: NULL.

### sub marpa_r_ref

```raku
sub marpa_r_ref(
    Marpa::Marpa-Recognizer $r
) returns Marpa::Marpa-Recognizer
```

Increases the reference count by 1. Not needed by most applications. Success value: The recognizer object, $r. Failure value: NULL.

### sub marpa_r_unref

```raku
sub marpa_r_unref(
    Marpa::Marpa-Recognizer $r
) returns Mu
```

Decreases the reference count by 1, destroying r once the reference count reaches zero. When r is destroyed, the reference count of its base grammar is decreased by one. If this takes the reference count of the base grammar to zero, it too is destroyed.

### sub marpa_r_start_input

```raku
sub marpa_r_start_input(
    Marpa::Marpa-Recognizer $r
) returns Int
```

Makes r ready to accept input. The first Earley set, the one at earleme 0, will be completed during this call. Because the call to marpa_r_start_input() completes an Earley set, it may generate events. For details about the events that may be generated during Earley set completion, see the description of the marpa_r_earleme_complete() method. Success value: A non-negative value. Failure value: −2.

### sub marpa_r_alternative

```raku
sub marpa_r_alternative(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Symbol-ID $token_id,
    Int $value,
    Int $length
) returns Int
```

Reads a token into r. The token will start at the current earleme. Libmarpa allows tokens to be ambiguous, to be of variable length and to overlap. token_id is the symbol ID of the token, which must be a terminal. length is the length of the token. value is an integer that represents the value of the token. In applications where the token's actual value is not an integer, it is expected that the application will use this value as a “virtual” value, perhaps finding the actual value by using value to index an array. value is not used inside Libmarpa — it is simply stored to be returned by the valuator as a convenience for the application. Some applications may prefer to track token values on their own, perhaps based on the earleme location and token_id, instead of using Libmarpa's token values. A value of 0 is reserved for a now-deprecated feature. Do not use it. For more details on that feature, see the section Valued and unvalued symbols. When marpa_r_alternative() is successful, the value of the furthest earleme is set to the greater of its value before the call, and current+length, where current is the value of the current earleme. The values of the current and latest earlemes are unchanged by calls to marpa_r_alternative(). Several error codes leave the recognizer in a fully recoverable state, allowing the application to retry the marpa_r_alternative() method. Retry is efficient, and quite useable as a parsing technique. The error code of primary interest from this point of view is MARPA-ERR-UNEXPECTED-TOKEN-ID, which indicates that the token was not accepted because of its token ID. Retry after this condition is used in several applications, and is called “the Ruby Slippers technique”. The error codes MARPA-ERR-DUPLICATE-TOKEN, MARPA-ERR-NO-TOKEN-EXPECTED-HERE and MARPA-ERR-INACCESSIBLE-TOKEN also leave the recognizer in a fully recoverable state, and may also be useable for the Ruby Slippers or similar techniques. At this writing, the author knows of no applications which attempt to recover from these errors. Success value: MARPA-ERR-NONE. Failure value: Some other error code.

### sub marpa_r_earleme_complete

```raku
sub marpa_r_earleme_complete(
    Marpa::Marpa-Recognizer $r
) returns Int
```

This method does the final processing for the current earleme. It then advances the current earleme by one. Note that marpa_r_earleme_complete() may be called even when no tokens have been read at the current earleme — in the character-per-earleme input model, for example, tokens can span many characters and, if the input is unambiguous over that span, there will be no other tokens that start inside it. As mentioned, marpa_r_earleme_complete() always advances the current earleme, incrementing its value by 1. This means that value of the current earleme after the call will be the one plus the value of the earleme processed by the call to marpa_r_earleme_complete(). If any token was accepted at the earleme being processed, marpa_r_earleme_complete() creates a new Earley set which will be the latest Earley set and, after the call, the latest earleme will be equal to the new current earleme. If no token was accepted at the earleme being processed, no Earley set is created, and the value of the latest earleme remains unchanged. The value of the furthest earleme is never changed by a call to marpa_r_earleme_complete(). During this method, one or more events may occur. On success, this function returns the number of events generated, but it is important to note that events may be created whether earleme completion fails or succeeds. When this method fails, the application must call marpa_g_event() if it wants to determine if any events occurred. Since the reason for failure to complete an earleme is often detailed in the events, applications that fail will often be at least as interested in the events as those that succeed. The MARPA-EVENT-EARLEY-ITEM-THRESHOLD event indicates that an application-settable threshold on the number of Earley items has been reached or exceeded. What this means depends on the application, but when the default threshold is exceeded, it means that it is very likely that the time and space resources consumed by the parse will prove excessive. A parse is “exhausted” when it can accept no more input. This can happen both on success and on failure. Note that the failure due to parse exhaustion only means failure at the current earleme. There may be successful parses at earlier earlemes. If a parse is exhausted, but successful, an event with the event code MARPA-EVENT-EXHAUSTED occurs. Because the parse is exhausted, no input will be accepted at later earlemes. It is quite common for a parse to become exhausted when it succeeds. Many practical grammars are designed so that a successful parse cannot be extended. An exhausted parse may cause a failure, in which case marpa_r_earleme_complete() returns an error whose error code is MARPA-ERR-PARSE-EXHAUSTED. For a parse to fail at an earleme due to exhaustion, it must be the case that no alternatives were accepted at that earleme. In fact, in the standard input model, a failure due to parse exhaustion occurs if and only if no alternatives were accepted at the current earleme. The circumstances under which failure due to parse exhaustion occurs are slightly more complicated when variable length tokens are in use. Informally, a parse will never fail due to exhaustion as long as it is possible that a token ending at some future earleme will continue the parse. More precisely, a call to marpa_r_earleme_complete() fails due to parse exhaustion if and only if, first, no alternatives were added at the current earleme and, second, that call left the current earleme equal to the furthest earleme. Success value: The number of events generated. Failure value: −2.

### sub marpa_r_current_earleme

```raku
sub marpa_r_current_earleme(
    Marpa::Marpa-Recognizer $r
) returns Marpa::Marpa-Earleme
```

Success value: If input has started, the current earleme. If input has not started, −1. Failure value: Always succeeds.

### sub marpa_r_earleme

```raku
sub marpa_r_earleme(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Earley-Set-ID $set_id
) returns Marpa::Marpa-Earleme
```

In the default, token-stream model, Earley set ID and earleme are always equal, but this is not the case in other input models. (The ID of an Earley set ID is also called its ordinal.) If there is no Earley set whose ID is set_id, marpa_r_earleme() fails. If set_id was negative, the error code is set to MARPA-ERR-INVALID-LOCATION. If set_id is greater than the ordinal of the latest Earley set, the error code is set to MARPA-ERR-NO-EARLEY-SET-AT-LOCATION. At this writing, there is no method for the inverse operation (conversion of an earleme to an Earley set ID). One consideration in writing such a method is that not all earlemes correspond to Earley sets. Applications that want to map earlemes to Earley sets will have no trouble if they are using the standard input model — the Earley set ID is always exactly equal to the earleme in that model. For other applications that want an earleme-to-ID mapping, the most general method is create an ID-to-earleme array using the marpa_r_earleme() method and invert it. Success value: The earleme corresponding to Earley set set_id. Failure value: −2.

### sub marpa_r_earley_set_value

```raku
sub marpa_r_earley_set_value(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Earley-Set-ID $earley_set
) returns Int
```

Returns the integer value of earley_set. For more details, see the description of marpa_r_earley_set_values(). Success value: The value of earley_set. Failure value: −2.

### sub marpa_r_earley_set_values

```raku
sub marpa_r_earley_set_values(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Earley-Set-ID $earley_set,
    NativeCall::Types::Pointer[Int] $p_value,
    NativeCall::Types::Pointer[NativeCall::Types::Pointer[NativeCall::Types::void]] $p_pvalue
) returns Int
```

If p_value is non-zero, sets the location pointed to by p_value to the integer value of the Earley set. Similarly, if p_pvalue is non-zero, sets the location pointed to by p_pvalue to the pointer value of the Earley set. The “value” and “pointer” of an Earley set are an arbitrary integer and an arbitrary pointer that the application can use for its own purposes. In character-per-earleme input models, for example, the integer can be the codepoint of the current character. In a traditional token-per-earleme input model, they could be used to indicate the string value of the token – the pointer could point to the start of the string, and the integer could indicate its length. The Earley set value and pointer can be set using the marpa_r_latest_earley_set_values_set() method. The Earley set integer value defaults to −1, and the pointer value defaults to NULL. Success value: returns a non-negative integer. Failure value: −2.

### sub marpa_r_furthest_earleme

```raku
sub marpa_r_furthest_earleme(
    Marpa::Marpa-Recognizer $r
) returns UInt
```

Success value: The furthest earleme. Failure value: Always succeeds.

### sub marpa_r_latest_earley_set

```raku
sub marpa_r_latest_earley_set(
    Marpa::Marpa-Recognizer $r
) returns Marpa::Marpa-Earley-Set-ID
```

This method returns the Earley set ID (ordinal) of the latest Earley set. Applications that want the value of the latest earleme can convert this value using the marpa_r_earleme() method. Success value: the ID of the latest Earley set. Failure value: Always succeeds.

### sub marpa_r_latest_earley_set_value_set

```raku
sub marpa_r_latest_earley_set_value_set(
    Marpa::Marpa-Recognizer $r,
    Int $value
) returns Int
```

Sets the integer value of the latest Earley set. For more details, see the description of marpa_r_latest_earley_set_values_set(). Success value: The new value of earley_set. Failure value: −2.

### sub marpa_r_latest_earley_set_values_set

```raku
sub marpa_r_latest_earley_set_values_set(
    Marpa::Marpa-Recognizer $r,
    Int $value,
    NativeCall::Types::Pointer[NativeCall::Types::void] $pvalue
) returns Int
```

Sets the integer and pointer value of the latest Earley set. For more about the “integer value” and “pointer value” of an Earley set, see the description of the marpa_r_earley_set_values() method. Success value: Returns a non-negative integer. Failure value: −2.

### sub marpa_r_completion_symbol_activate

```raku
sub marpa_r_completion_symbol_activate(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $reactivate
) returns Int
```

Allows the user to deactivate and reactivate symbol completion events in the recognizer. If reactivate is zero, the event is deactivated. If reactivate is one, the event is activated. Symbol completion events are active by default if the symbol was set up for completion events in the grammar. If a symbol was not set up for completion events in the grammar, symbol completion events are inactive by default and any attempt to change that is a fatal error. Success value: The method returns the value of reactivate. The method succeeds trivially if the symbol is already set as indicated by reactivate. Failure value: If the active status of the completion event for sym_id cannot be set as indicated by reactivate, the method fails. On failure, −2 is returned.

### sub marpa_r_earley_item_warning_threshold_set

```raku
sub marpa_r_earley_item_warning_threshold_set(
    Marpa::Marpa-Recognizer $r,
    Int $threshold
) returns Int
```

These methods, respectively, set and query the Earley item warning threshold. The Earley item warning threshold is a number that is compared with the count of Earley items in each Earley set. When it is matched or exceeded, a MARPA-EVENT-EARLEY-ITEM-THRESHOLD event is created. If threshold is zero or less, an unlimited number of Earley items will be allowed without warning. This will rarely be what the user wants. By default, Libmarpa calculates a value based on the grammar. The formula Libmarpa uses is the result of some experience, and most applications will be happy with it. Success value: The value that the Earley item warning threshold has after the method call is finished. Failure value: Always succeeds.

### sub marpa_r_expected_symbol_event_set

```raku
sub marpa_r_expected_symbol_event_set(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Symbol-ID $symbol_id,
    Int $value
) returns Int
```

Sets the “expected symbol event bit” for symbol_id to value. A recognizer event is created whenever symbol symbol_id is expected at the current earleme. if and only if the expected symbol event bit for symbol_id is 1. The “expected symbol event bit” must be 1 or 0. In this context, “expected” means “expected as a terminal”. Even if a symbol is predicted at the current earleme, if it is not acceptable as a terminal, it does not trigger an “expected symbol event”. By default, the “expected symbol event bit” is 0. It is an error to attempt to set the “expected symbol event bit” to 1 for a nulling symbol, an inaccessible symbol, or an unproductive symbol. Success value: The value of the event bit after the method call is finished. Failure value: -2 if symbol_id is not the ID of a valid symbol; if it is the ID of an nulling, inaccessible for unproductive symbol; or on other failure.

### sub marpa_r_is_exhausted

```raku
sub marpa_r_is_exhausted(
    Marpa::Marpa-Recognizer $r
) returns Int
```

A parser is “exhausted” if it cannot accept any more input. Both successful and failed parses can be exhausted. In many grammars, the parse is always exhausted as soon as it succeeds. Good parses may also exist at earlemes prior to the current one. Success value: 1 if the parser is exhausted, 0 otherwise. Failure value: Always succeeds.

### sub marpa_r_nulled_symbol_activate

```raku
sub marpa_r_nulled_symbol_activate(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $boolean
) returns Int
```

Allows the user to deactivate and reactivate symbol nulled events in the recognizer. If boolean is zero, the event is deactivated. If boolean is one, the event is activated. Symbol nulled events are active by default if the symbol was set up for nulled events in the grammar. If a symbol was not set up for nulled events in the grammar, symbol nulled events are inactive by default and any attempt to change that is a fatal error. Success cases: On success, the method returns the value of boolean. The method succeeds trivially if the symbol is already set as indicated by boolean. Failure cases: If the active status of the nulled event for sym_id cannot be set as indicated by boolean, the method fails. On failure, −2 is returned.

### sub marpa_r_prediction_symbol_activate

```raku
sub marpa_r_prediction_symbol_activate(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $boolean
) returns Int
```

Allows the user to deactivate and reactivate symbol prediction events in the recognizer. If boolean is zero, the event is deactivated. If boolean is one, the event is activated. Symbol prediction events are active by default if the symbol was set up for prediction events in the grammar. If a symbol was not set up for prediction events in the grammar, symbol prediction events are inactive by default and any attempt to change that is a fatal error. Success value: The method returns the value of boolean. The method succeeds trivially if the symbol is already set as indicated by boolean. Failure value: If the active status of the prediction event for sym_id cannot be set as indicated by boolean, the method fails. On failure, −2 is returned.

### sub marpa_r_terminals_expected

```raku
sub marpa_r_terminals_expected(
    Marpa::Marpa-Recognizer $r,
    NativeCall::Types::Pointer[Marpa::Marpa-Symbol-ID] $buffer
) returns Int
```

Returns a list of the ID's of the symbols that are acceptable as tokens at the current earleme. buffer is expected to be large enough to hold the result. This is guaranteed to be the case if the buffer is large enough to hold a number of Marpa-Symbol-ID's that is greater than or equal to the number of symbols in the grammar. Success value: The number of Marpa-Symbol-ID's in buffer. Failure value: −2.

### sub marpa_r_terminal_is_expected

```raku
sub marpa_r_terminal_is_expected(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Symbol-ID $symbol_id
) returns Int
```

Success value: If symbol_id is the ID of a valid terminal symbol that is expected at the current earleme, a number greater than zero. If symbol_id is the ID of a valid terminal symbol that is not expected at the current earleme, or if symbol_id is the ID of a valid symbol that is not a terminal, zero. Failure value: −2. It is a failure if symbol_id is not the ID of a valid symbol.

### sub marpa_r_progress_report_reset

```raku
sub marpa_r_progress_report_reset(
    Marpa::Marpa-Recognizer $r
) returns Int
```

Resets the progress report. Assumes a report of the progress has already been initialized at some Earley set for recognizer r, with marpa_r_progress_report_start(). The reset progress report will be positioned before its first item. Success value: A non-negative value. Failure value: −2.

### sub marpa_r_progress_report_start

```raku
sub marpa_r_progress_report_start(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Earley-Set-ID $set_id
) returns Int
```

Initializes a report of the progress at Earley set set_id for recognizer r. If a progress report already exists, it is destroyed and its memory is freed. Initially, the progress report is positioned before its first item. If no Earley set with ID set_id exists, marpa_r_progress_report_start() fails. The error code is MARPA-ERR-INVALID-LOCATION if set_id is negative. The error code is MARPA-ERR-NO-EARLEY-SET-AT-LOCATION if set_id is greater than the ID of the latest Earley set. Success value: the number of report items available. If the recognizer has not been started; if set_id does not exist; or on other failure, −2. Failure value: −2.

### sub marpa_r_progress_report_finish

```raku
sub marpa_r_progress_report_finish(
    Marpa::Marpa-Recognizer $r
) returns Int
```

Destroys the report of the progress at Earley set set_id for recognizer r, freeing the memory and other resources. It is often not necessary to call this method. Any previously existing progress report is destroyed automatically whenever a new progress report is started, and when the recognizer is destroyed. Success value: A non-negative value. Failure value: −2 if no progress report has been started, or on other failure.

### sub marpa_r_progress_item

```raku
sub marpa_r_progress_item(
    Marpa::Marpa-Recognizer $r,
    NativeCall::Types::Pointer[Int] $position,
    NativeCall::Types::Pointer[Marpa::Marpa-Earley-Set-ID] $origin
) returns Marpa::Marpa-Rule-ID
```

This method allows access to the data for the next item of a progress report. If there are no more progress report items, it returns −1 as a termination indicator and sets the error code to MARPA-ERR-PROGRESS-REPORT-EXHAUSTED. Either the termination indicator, or the item count returned by marpa_r_progress_report_start(), can be used to determine when the last item has been seen. On success, the dot position is returned in the location pointed to by the position argument, and the origin is returned in the location pointed to by the origin argument. On failure, the locations pointed to by the position and origin arguments are unchanged. Success value: The rule ID of the next progress report item. If there are no more progress report items, −1. If either the position or the origin argument is NULL, or on other failure, −2. Failure value: −2.

### sub marpa_b_new

```raku
sub marpa_b_new(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Earley-Set-ID $earley_set_ID
) returns Marpa::Marpa-Bocage
```

Creates a new bocage object, with a reference count of 1. The reference count of its parent recognizer object, r, is increased by 1. If earley_set_ID is −1, the Earley set at the current earleme is used, if there is one. If earley_set_ID is −1 and there is no Earley set at the current earleme; or if earley_set_ID is −1 and there is no parse ending at Earley set earley_set_ID, marpa_b_new() fails and the error code is set to MARPA-ERR-NO-PARSE. Success value: The new bocage object. Failure value: NULL.

### sub marpa_b_ref

```raku
sub marpa_b_ref(
    Marpa::Marpa-Bocage $b
) returns Marpa::Marpa-Bocage
```

Increases the reference count by 1. Not needed by most applications. Success value: $b. Failure value: NULL.

### sub marpa_b_unref

```raku
sub marpa_b_unref(
    Marpa::Marpa-Bocage $b
) returns Mu
```

Decreases the reference count by 1, destroying b once the reference count reaches zero. When b is destroyed, the reference count of its parent recognizer is decreased by 1. If this takes the reference count of the parent recognizer to zero, it too is destroyed. If the parent recognizer is destroyed, the reference count of its base grammar is decreased by 1. If this takes the reference count of the base grammar to zero, it too is destroyed.

### sub marpa_b_ambiguity_metric

```raku
sub marpa_b_ambiguity_metric(
    Marpa::Marpa-Bocage $b
) returns Int
```

Returns an ambiguity metric. The metric is 1 is the parse is unambiguous. If the metric is 2 or greater, the parse is ambiguous. It was originally intended to have values greater than 2 be an cheaply computed estimate of the degree of ambiguity, but a satisfactory scheme for this has yet to be implemented. Success value: 1 if the bocage is not for an ambiguous parse; 2 or greater if the bocage is for an ambiguous parse. Failure value: −2.

### sub marpa_b_is_null

```raku
sub marpa_b_is_null(
    Marpa::Marpa-Bocage $b
) returns Int
```

Success value: A number greater than or equal to 1 if the bocage is for a null parse; otherwise, 0. Failure value: −2.

### sub marpa_o_new

```raku
sub marpa_o_new(
    Marpa::Marpa-Bocage $b
) returns Marpa::Marpa-Order
```

Creates a new ordering object, with a reference count of 1. The reference count of its parent bocage object, b, is increased by 1. Success value: the new ordering object. Failure value: NULL.

### sub marpa_o_ref

```raku
sub marpa_o_ref(
    Marpa::Marpa-Order $o
) returns Marpa::Marpa-Order
```

Increases the reference count by 1. Not needed by most applications. Success value: o. Failure value: NULL.

### sub marpa_o_unref

```raku
sub marpa_o_unref(
    Marpa::Marpa-Order $o
) returns Mu
```

Decreases the reference count by 1, destroying o once the reference count reaches zero. Beginning with o's parent bocage, Libmarpa then proceeds up the chain of parent objects. Every time a child is destroyed, the reference count of its parent is decreased by 1. Every time the reference count of an object is decreased by 1, if that reference count is now zero, that object is destroyed. Libmarpa follows this chain of decrements and destructions as required, all the way back to the base grammar, if necessary.

### sub marpa_o_ambiguity_metric

```raku
sub marpa_o_ambiguity_metric(
    Marpa::Marpa-Order $o
) returns Int
```

Returns an ambiguity metric. The metric is 1 is the parse is unambiguous. If the metric is 2 or greater, the parse is ambiguous. It was originally intended to have values greater than 2 be an cheaply computed estimate of the degree of ambiguity, but a satisfactory scheme for this has yet to be implemented. If the ordering is not already frozen, it will be frozen on return from marpa_o_ambiguity_metric(). marpa_o_ambiguity_metric() is considered an “accessor”, because it is assumed that the ordering is frozen when marpa_o_ambiguity_metric() is called. Success value: 1 if the ordering is not for an ambiguous parse; 2 or greater if the ordering is for an ambiguous parse. Failure value: −2.

### sub marpa_o_is_null

```raku
sub marpa_o_is_null(
    Marpa::Marpa-Order $o
) returns Int
```

Success value: A number greater than or equal to 1 if the ordering is for a null parse; otherwise, 0. Failure value: −2.

### sub marpa_o_high_rank_only_set

```raku
sub marpa_o_high_rank_only_set(
    Marpa::Marpa-Order $o,
    Int $flag
) returns Int
```

These methods, respectively, set and query the “high rank only” flag of ordering o. A flag of 1 indicates that, when ranking, all choices should be discarded except those of the highest rank. A flag of 0 indicates that no choices should be discarded on the basis of their rank. A value of 1 is the default. The value of the “high rank only” flag has no effect unless ranking has been turned on using the marpa_o_rank() method. Success value: the value of the “high rank only” flag after the call. Failure value: −2.

### sub marpa_o_rank

```raku
sub marpa_o_rank(
    Marpa::Marpa-Order $o
) returns Int
```

By default, the ordering of parse trees is arbitrary. This method causes the ordering to be ranked according to the ranks of symbols and rules, the “null ranks high” flags of the rules, and the “high rank only” flag of the ordering. Once this method returns, the ordering is frozen. Success value: A non-negative value. Failure value: −2.

### sub marpa_t_new

```raku
sub marpa_t_new(
    Marpa::Marpa-Order $o
) returns Marpa::Marpa-Tree
```

Creates a new tree iterator, with a reference count of 1. The reference count of its parent ordering object, o, is increased by 1. When initialized, a tree iterator is positioned before the first parse tree. To position the tree iterator to the first parse, the application must call marpa_t_next(). Success value: A newly created tree. Failure value: NULL.

### sub marpa_t_ref

```raku
sub marpa_t_ref(
    Marpa::Marpa-Tree $t
) returns Marpa::Marpa-Tree
```

Increases the reference count by 1. Not needed by most applications. Success value: t. Failure value: NULL.

### sub marpa_t_unref

```raku
sub marpa_t_unref(
    Marpa::Marpa-Tree $t
) returns Mu
```

Decreases the reference count by 1, destroying t once the reference count reaches zero. Beginning with t's parent ordering, Libmarpa then proceeds up the chain of parent objects. Every time a child is destroyed, the reference count of its parent is decreased by 1. Every time the reference count of an object is decreased by 1, if that reference count is now zero, that object is destroyed. Libmarpa follows this chain of decrements and destructions as required, all the way back to the base grammar, if necessary.

### sub marpa_t_next

```raku
sub marpa_t_next(
    Marpa::Marpa-Tree $t
) returns Int
```

Positions t at the next parse tree in the iteration. Tree iterators are initialized to the position before the first parse tree, so this method must be called before creating a valuator from a tree. If a tree iterator is positioned after the last parse, the tree is said to be “exhausted”. A tree iterator for a bocage with no parse trees is considered to be “exhausted” when initialized. If the tree iterator is exhausted, marpa_t_next() returns −1 as a termination indicator, and sets the error code to MARPA-ERR-TREE-EXHAUSTED. Success value: A non-negative value. If the tree iterator is exhausted, −1. Failure value: −2.

### sub marpa_t_parse_count

```raku
sub marpa_t_parse_count(
    Marpa::Marpa-Tree $t
) returns Int
```

The parse counter counts the number of parse trees traversed so far. The count includes the current iteration of the tree, so that a value of 0 indicates that the tree iterator is at its initialized position, before the first parse tree. Success value: The number of parses traversed so far. Failure value: Always succeeds.

### sub marpa_v_new

```raku
sub marpa_v_new(
    Marpa::Marpa-Tree $t
) returns Marpa::Marpa-Value
```

Creates a new valuator. The parent object of the new valuator will be the tree iterator t, and the reference count of the new valuator will be 1. The reference count of t is increased by 1. The parent tree iterator is “paused”, so that the tree iterator cannot move on to a new parse tree until the valuator is destroyed. Many valuators of the same parse tree can exist at once. A tree iterator is “unpaused” when all of the valuators of that tree iterator are destroyed. Success value: The newly created valuator. Failure value: NULL.

### sub marpa_v_ref

```raku
sub marpa_v_ref(
    Marpa::Marpa-Value $v
) returns Marpa::Marpa-Value
```

Increases the reference count by 1. Not needed by most applications. Success value: v. Failure value: NULL.

### sub marpa_v_unref

```raku
sub marpa_v_unref(
    Marpa::Marpa-Value $v
) returns Mu
```

Decreases the reference count by 1, destroying v once the reference count reaches zero. Beginning with v's parent tree, Libmarpa then proceeds up the chain of parent objects. Every time a child is destroyed, the reference count of its parent is decreased by 1. Every time the reference count of an object is decreased by 1, if that reference count is now zero, that object is destroyed. Libmarpa follows this chain of decrements and destructions as required, all the way back to the base grammar, if necessary.

### sub marpa_v_step

```raku
sub marpa_v_step(
    Marpa::Marpa-Value $v
) returns Marpa::Marpa-Step-Type
```

This method “steps through” the valuator. The return value is a Marpa-Step_Type, an integer which indicates the type of step. How the application is expected to act on each step is described below. See Valuator steps by type. When the iteration through the steps is finished, marpa_v_step returns MARPA-STEP-INACTIVE. Success value: The type of the step to be performed. This will always be a non-negative number. Failure value: −2.

### sub marpa_g_event

```raku
sub marpa_g_event(
    Marpa::g $g,
    NativeCall::Types::Pointer[Marpa::Marpa-Event] $event,
    Int $ix
) returns Marpa-Event-Code
```

On success, the type of the ix'th event is returned and the data for the ix'th event is placed in the location pointed to by event. Event indexes are in sequence. Valid events will be in the range from 0 to n, where n is one less than the event count. The event count can be queried using the marpa_g_event_count() method. Success value: The type of event ix. If there is no ix'th event, if ix is negative, or on other failure, −2. Failure value: −2. The locations pointed to by event are not changed.

### sub marpa_g_event_count

```raku
sub marpa_g_event_count(
    Marpa::g $g
) returns Int
```

Success value: The number of events. Failure value: −2.

### sub marpa_g_error

```raku
sub marpa_g_error(
    Marpa::g $g,
    NativeCall::Types::Pointer[Str] $p_error_string
) returns Marpa-Error-Code
```

When a method fails, this method allows the application to read the error code. p_error_string is reserved for use by the internals. Applications should set it to NULL. Success value: The last error code from a Libmarpa method. Failure value: Always succeeds.

### sub marpa_g_error_clear

```raku
sub marpa_g_error_clear(
    Marpa::g $g
) returns Marpa-Error-Code
```

Sets the error code to MARPA-ERR-NONE. Not often used, but now and then it can be useful to force the error code to a known state. Success value: MARPA-ERR-NONE. Failure value: Always succeeds.

### sub marpa_g_default_rank_set

```raku
sub marpa_g_default_rank_set(
    Marpa::g $g,
    Marpa::Marpa-Rank $rank
) returns Marpa::Marpa-Rank
```

These methods, respectively, set and query the default rank of the grammar. When a grammar is created, the default rank is 0. When rules and symbols are created, their rank is the default rank of the grammar. Changing the grammar's default rank does not affect those rules and symbols already created, only those that will be created. This means that the grammar's default rank can be used to, in effect, assign ranks to groups of rules and symbols. Applications may find this behavior useful. Success value: Returns the rank after the call, and sets the error code to MARPA-ERR-NONE. On failure, returns −2, and sets the error code to an appropriate value, which will never be MARPA-ERR-NONE. Note that when the rank is −2, the error code is the only way to distinguish success from failure. The error code can be determined by using the marpa_g_error() call. Failure value: −2.

### sub marpa_g_symbol_rank_set

```raku
sub marpa_g_symbol_rank_set(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $sym_id,
    Marpa::Marpa-Rank $rank
) returns Marpa::Marpa-Rank
```

These methods, respectively, set and query the rank of a symbol sym_id. When sym_id is created, its rank initialized to the default rank of the grammar. Success value: Returns the rank after the call, and sets the error code to MARPA-ERR-NONE. On failure, returns −2, and sets the error code to an appropriate value, which will never be MARPA-ERR-NONE. Note that when the rank is −2, the error code is the only way to distinguish success from failure. The error code can be determined by using the marpa_g_error() call. Failure value: −2.

### sub marpa_g_zwa_new

```raku
sub marpa_g_zwa_new(
    Marpa::g $g,
    Int $default_value
) returns Marpa::Marpa-Assertion-ID
```

On success, returns previous default value of the assertion.

### sub marpa_r_zwa_default_set

```raku
sub marpa_r_zwa_default_set(
    Marpa::Marpa-Recognizer $r,
    Marpa::Marpa-Assertion-ID $zwaid,
    Int $default_value
) returns Int
```

Changes default value to default_value. On success, returns previous default value of the assertion.

### sub marpa_g_symbol_is_valued_set

```raku
sub marpa_g_symbol_is_valued_set(
    Marpa::g $g,
    Marpa::Marpa-Symbol-ID $symbol_id,
    Int $value
) returns Int
```

These methods, respectively, set and query the “valued status” of a symbol. Once set to a value with the marpa_g_symbol_is_valued_set() method, the valued status of a symbol is “locked” at that value. It cannot thereafter be changed. Subsequent calls to marpa_g_symbol_is_valued_set() for the same sym_id will fail, leaving sym_id's valued status unchanged, unless value is the same as the locked-in value. Success value: 1 if the symbol symbol_id is valued after the call, 0 if not. If the valued status is locked and value is different from the current status, −2. If value is not 0 or 1; or on other failure, −2. Failure value: −2.

### sub marpa_v_symbol_is_valued_set

```raku
sub marpa_v_symbol_is_valued_set(
    Marpa::Marpa-Value $v,
    Marpa::Marpa-Symbol-ID $sym_id,
    Int $status
) returns Int
```

These methods, respectively, set and query the valued status of symbol sym_id. marpa_v_symbol_is_valued_set() will set the valued status to the value of its status argument. A valued status of 1 indicates that the symbol is valued. A valued status of 0 indicates that the symbol is unvalued. If the valued status is locked, an attempt to change to a status different from the current one will fail (error code MARPA-ERR-VALUED-IS-LOCKED). Success value: The valued status after the call. If value is not either 0 or 1, or on other failure, −2. Failure value: −2.

### sub marpa_v_rule_is_valued_set

```raku
sub marpa_v_rule_is_valued_set(
    Marpa::Marpa-Value $v,
    Marpa::Marpa-Rule-ID $rule_id,
    Int $status
) returns Int
```

These methods, respectively, set and query the valued status for the LHS symbol of rule rule_id. marpa_v_rule_is_valued_set() sets the valued status to the value of its status argument. A valued status of 1 indicates that the symbol is valued. A valued status of 0 indicates that the symbol is unvalued. If the valued status is locked, an attempt to change to a status different from the current one will fail (error code MARPA-ERR-VALUED-IS-LOCKED). Rules have no valued status of their own. The valued status of a rule is always that of its LHS symbol. These methods are conveniences — they save the application the trouble of looking up the rule's LHS. Success value: The valued status of the rule rule_id's LHS symbol after the call. If value is not either 0 or 1, or on other failure, −2. Failure value: −2.

### sub marpa_v_valued_force

```raku
sub marpa_v_valued_force(
    Marpa::Marpa-Value $v
) returns Int
```

This methods locks the valued status of all symbols to 1, indicated that the symbol is valued. If this is not possible, for example because one of the grammar's symbols already is locked at a valued status of 0, failure is returned. Success value: A non-negative number. Failure value: −2, and sets the error code to an appropriate value, which will never be MARPA-ERR-NONE.

