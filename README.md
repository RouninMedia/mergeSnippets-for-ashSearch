# mergeSnippets for ashSearch
**`mergeSnippets()`** intelligently concatenates overlapping search snippets identified by `ashSearch`.

In an array of elements, each element represents an individual snippet.

When applied to this array:

```
$My_Array = [

  'The quick',
  'quick brown',
  'quick brown fox',
  'jumps over the',
  'over the',
  'lazy dog',
];
```

**`mergeSnippets($Snippets_Array)`** will return:

```
$My_Processed_Array = [

  'The quick brown fox',
  'jumps over the',
  'lazy dog',
];
```


____

## `mergeSnippets()` function

```

function mergeSnippets($Snippets_Array) {
    
  for ($i = (count($Snippets_Array) - 1); $i > 0; $i--) {
  
    // TURN STRING ELEMENTS INTO MINI-ARRAYS
    $Current_Element = explode(' ', trim($Snippets_Array[$i]));
    $Previous_Element = explode(' ', trim($Snippets_Array[($i - 1)]));
    
    $End_Loop = FALSE;
    
    // STRING-MATCHING ROUTINE
    while ($End_Loop === FALSE) {

      if ($Current_Element[0] === $Previous_Element[(count($Previous_Element) - 1)]) {            
        array_shift($Current_Element);
        $Snippets_Array[$i] = implode(' ', $Current_Element);
        $Snippets_Array[($i - 1)] .= ' '.$Snippets_Array[$i];
        unset($Snippets_Array[$i]);
        $Snippets_Array = array_values($Snippets_Array);
        
        $End_Loop = TRUE;
      }
        
      elseif (count($Current_Element) > 1) {
        $Current_Element[0] .= ' '.$Current_Element[1];
        unset($Current_Element[1]);
        $Current_Element = array_values($Current_Element);
      
        if (isset($Previous_Element[(count($Previous_Element) - 2)])) {
          $Previous_Element[(count($Previous_Element) - 2)] .= ' '.$Previous_Element[(count($Previous_Element) - 1)];
          unset($Previous_Element[(count($Previous_Element) - 1)]);
          $Previous_Element = array_values($Previous_Element);
        }
      }
      
      elseif (count($Current_Element) === 1) {
        $End_Loop = TRUE;
      }
    }
  }
    
  return $Snippets_Array;
}

```
____

## Background Information...

### What is `ashSearch`?
`ashSearch` is the standard Search Module in `ash`.


### What is `ash`?

`ash` is a standard library of `da3sh`-based rich web components (or *modules*) which can be deployed across all `da3sh`-parsing environments (such as `ashiva`).
