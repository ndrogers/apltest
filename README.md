# APLTEST

A simple test reporting tool with examples. For complete example usage see `./meta_test.aplf` as it runs every possible combination of the test function. 

## Usage

To use this project:

```APL
    ]link.create # 'path/to/apltest' 
Linked: # ←→ path/to/apltest
```

The entry point to this test reporting tool is `./test.aplf` 

To execute all test cases in this project:
```APL
    test ⍬
┌─────────────┬─────────────┬─────────────────────────────────────────────────────────────┐
│AOC2015 FNS  │REPORT       │DESCRIPTION                                                  │ 
├─────────────┼─────────────┼─────────────────────────────────────────────────────────────┤
│s01          │┌────────┬──┐│PASS                                                         │
│             ││RAN:    │10││                                                             │
│             │├────────┼──┤│                                                             │
│             ││PASSED: │10││                                                             │
│             │└────────┴──┘│                                                             │
├─────────────┼─────────────┼─────────────────────────────────────────────────────────────┤
│s02          │┌────────┬─┐ │PASS                                                         │
│             ││RAN:    │2│ │                                                             │
│             │├────────┼─┤ │                                                             │
│             ││PASSED: │2│ │                                                             │
│             │└────────┴─┘ │                                                             │
├─────────────┼─────────────┼─────────────────────────────────────────────────────────────┤
│s03          │┌────────┬─┐ │┌──────────┬──────────┬──────────┬──────────┐                │
│             ││RAN:    │5│ ││Input:   1│Input:   1│Input:   1│Input:   1│                │
│             │├────────┼─┤ │├──────────┼──────────┼──────────┼──────────┤                │
│             ││PASSED: │1│ ││Expected:5│Expected:6│Expected:7│Expected:8│                │
│             │├────────┼─┤ │├──────────┼──────────┼──────────┼──────────┤                │
│             ││FAILED: │4│ ││Got:     2│Got:     2│Got:     2│Got:     2│                │
.............................................................................................
│             │             ││Got:     92│Got:     2│Got:     20│Got:     91│Got:     9  ││
│             │             │└───────────┴──────────┴───────────┴───────────┴────────────┘│
└─────────────┴─────────────┴─────────────────────────────────────────────────────────────┘ 
    
```



|RESULT|⍺|⍵|Example&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
|---|---|---|---|
|Run every dfn defined in each namespace in `#` with their corresponding test cases||⍬|`test ⍬`|
|Run every defn defined in ⍵ with their corresponding test cases||namespace|`test #.aoc2015`|
|Run the dfns ⍵ defined in ⍺|namespace|nested charvec|`#.aoc2015 test 's01' 's02`|
|Run dfn ⍵ in ⍺|namespace|charvec|`#.aoc2015 test 's01'`|

## Defining your own tests and test cases

Create a new file named `my_namespace.apln` containing:
```APL
:Namespace my_namespace
    
:EndNamespace
```

Inside that namespace we'll define a function, and a corresponding test case. Test cases are identified by appending `_tests` to the name of a given function.

```APL
my_fn←{}

my_fn_tests←⍬
```

This test will not execute because there are no test cases, and my_fn returns no results so calling it will probably result in an error. 

To defined our first case, we'll change the values to the following:
```APL
my_fn←{+/⍵}
my_fn_tests←(1 1)((1 2) 3)((2 8 20) 30)
```

Each cell is a tuple containing strictly 2 values. First is the input, second is the expected result.

Now call `test` to run this test:

```APL
    test #.my_namespace         ⍝ will run all test cases in the namespace
    
    #.my_namespace test 'my_fn' ⍝ will only run my_fn_tests
┌──────────────────┬────────────┬───────────┐
│#.MY_NAMESPACE FNS│REPORT      │DESCRIPTION│
├──────────────────┼────────────┼───────────┤
│my_fn             │┌────────┬─┐│PASS       │
│                  ││RAN:    │3││           │
│                  │├────────┼─┤│           │
│                  ││PASSED: │3││           │
│                  │└────────┴─┘│           │
└──────────────────┴────────────┴───────────┘
```


## Precautions 
- At present, this framework only allows for dfns accepting a right argument. The framework also presently only tests dfns defined in the provided namespace, and does not recursively check for more nested namespaces to test. 

- Errors caused by function execution are trapped, so any embedded error handling won't be handled or tested. 

- No mocking is provided. Any dfn that requires user interaction, or anything depending on outside data isn't guaranteed. 
