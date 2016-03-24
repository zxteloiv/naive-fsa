# naive_fsa
A naive implementation of Finite State Automata

### Usage

Actually the codes do less, and the data structures do much more. All the automata graph is given in the structure.

The overall graph is a dict object. Take the following one as an example:

``` python

states = {
        "start": {
            # a state without "default" doesn't match anything if no known symbol is encountered
            "trans": [('a', 'first'), ('b', 'second')],
            },
        "first": {
            # a state with re pattern symbol will use the pattern to match remaining
            "trans": [(re.compile('ef'), 'second')]
            },
        "second": {
            # a state with "default" goes there if no known symbol is encountered
            "default": "first",
            "trans": [('3', 'third')]
            },
        "third": {
            # a state with "direct" key goes directly to that state
            "direct": "end"
            },
        "end": {
            }
        }

from naive_fsa import FiniteStateAutomata

fsa = FiniteStateAutomata(states, start="start", end="end")

# continue to do sth. as: fsa.match(string, startpos)

```

Every node or state in the graph is a key-val pair of the dict.

For every state, it has a name, which is the key name, and a value of a dict object again.

Basically the state dict have the following keys:

- direct: key name string of the next state, which means the state will move to the state directly, without reading any more symbol from the input
- default: key name string of the next state, indicate the next state to move to, when the incoming symbol is not in the transition table
- trans: an ordered list of transient symbols. every item is a (symbol, state name) pair, if the symbol is matched with the string, move to the next indicated state. If the item does not match, move on to the next item in the list. A symbol could be of two types:
    - a simple string
    - a compiled regular expression object

If a _direct_ key is provided, no symbol in the transition table will be checked. The _default_ and _trans_ could exist at the same time by design.

### License

The Star And Thank Author License (SATA)
