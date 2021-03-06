10/18/2017

# Recursive backtracking
-***backtracking*** -- try partial solutions and then abandoning them if they are not suitable.
: vs. exhaustive search, which just finds all solutions. backtracking *rates* each one/solution instead.

## ex: dice rolls
- print all possible outcomes of rolling.
~~~~c++
void diceRollsHelper(int dice, Vector<int>& chosen) {
    if (dice == 0) {
        cout << chosen;
    }
    else {
        for (int i = 0; i <=6; i++) {
            chosen.add(i); // choose
            diceRollsHelper(dice - 1, chosen); // search/explore
            chosen.remove(chosen.size() - 1); // un-choose
        }
    }
}
void diceRolls(int dice) {
    Vector<int> v;
    diceRollsHelper(dice, v);
}
~~~~
Now, diceSum -- print all possible outcomes of rolling given # that add up to the sum.
**INEFFICENT**
```c++
        if (sumAll(chosen) == desiredSum) {
            cout << chosen << endl;   
        }
```
Optimizations:
1) computing sum over and over is wasteful
2) don't explore decision trees where values ...
1555 -> 193 calls!
~~~c++
void diceRollsHelper(int dice, int desiredSum, int mySumSoFar, Vector<int>& chosen) {
    if (dice == 0) {
        if (mySumSoFar == desiredSum) {
            cout << chosen << endl;
        }
    }
    else {
        for (int i = 1; i <= 6; i++) {
            if (mySumSoFar + i + 6*(dice - 1) < desiredSum || // max
                    mySumSoFar + i + 1*(dice - 1) > desiredSum // min
                    ) {
                continue;
            }
            chosen.add(i); // choose
            diceRollsHelper(dice - 1, desiredSum, mySumSoFar + i, chosen); // search/explore
            chosen.remove(chosen.size() - 1); // un-choose
        }
    }
}
void diceSum(int dice, int desiredSum) {
    Vector<int> v;
    diceRollsHelper(dice, desiredSum, 0, v);
}
~~~

## Exercise: sublists.
Find every possible sub-list of a given vector (basically, all possible subsets).

10/20/2017

# The "8 Queens" problem
Can you place 8 queens on the board such that no queen can attack another queen?
~~~~

~~~~

# "travel" problem
~~~~c++
void travelHelper(const GPoint& target, GPoint me, Vector<string>& chosen) {
    if (target == me) {
        // done
        cout << chosen << endl;
    } else if (me.getX() <= target.getX() &&
                me.getY() <= target.getY()) {
        // recursive case: take a step towards target
        GPoint e(me.getX() + 1, me.getY());
        GPoint n(me.getX() + 1, me.getY());
        GPoint ne(me.getX() + 1, me.getY() + 1);
        
        chosen.add("E");
        travelHelper(target, e, chosen);
        chosen.remove(chosen.size() - 1);
        
        // rinse and repeat for n and ne
    }
}
void travel(const GPoint& target) {
    GPoint origin(0, 0);
    travelHelper(target, origin, steps);
}
~~~~