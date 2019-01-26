---
title: Random Number Generator
date: 2014/01/14 15:29:00 +530
layout: single
comments: true
categories: 
   - .Net
tags:
   - linq
   - csharp
---

.Net has a very useful class called “Random” which generates a random number between a range. You can read about the Random class [here](http://msdn.microsoft.com/en-us/library/system.random(v=vs.110).aspx).

But one problem with the random class in C# is that it would give up duplicates every now and then. There are scenarios when you would want a set of numbers within a range to be shuffled or randomized.

i.e if the range is between 0 – 5 you would want {5,0,2,1,4,3} or some set that would be random and within the limits and end up not giving duplicates.

following is the code which generates the random numbers and can be iterated over (as it implements IEnumerable).

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

namespace RandomGenerators
{
    public class RandomNumbers : IEnumerable<int>,IEnumerable
    {
        private bool[] _alreadyUsedNumbers;
        private int[] _randomNumbers;
        private RandomNumberRange _range;
        private long _numbersGeneratedSofar = 0;
        Random randomGenerator = new Random();
        
        public RandomNumbers(int minValueInclusive, int maxInclusiveValue)
            : this(new RandomNumberRange(minValueInclusive,maxInclusiveValue)){}

        public RandomNumbers(RandomNumberRange range)
        {
            if (range == null)
                throw new ApplicationException("Range cannot be null");

            this._range = range;
            this._alreadyUsedNumbers = new bool[_range.NumbersToGenerate];
            this._randomNumbers = new int[_range.NumbersToGenerate];
            this.randomGenerator = new Random();

            GenerateRandomNumbers();
        }

        private void GenerateRandomNumbers()
        {
            while (_numbersGeneratedSofar < _range.NumbersToGenerate)
            {
                int numberGenerated = 0;
                do
                {
                    numberGenerated = randomGenerator.Next(_range.Minimum, _range.Maximum + 1);
                } while (_alreadyUsedNumbers[numberGenerated - _range.Minimum]);

                _alreadyUsedNumbers[numberGenerated - _range.Minimum] = true;
                _randomNumbers[_numbersGeneratedSofar++] = numberGenerated;
            }

            //reset the bool array
            _alreadyUsedNumbers = null;
        }

        #region Enumerable
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _randomNumbers.GetEnumerator();
        }

        IEnumerator<int> IEnumerable<int>.GetEnumerator()
        {
            for (int i = 0; i < _randomNumbers.Length; i++)
            {
                yield return _randomNumbers[i];
            }
        }

        #endregion
    }

    public class RandomNumberRange
    {
        public readonly int Minimum;
        public readonly int Maximum;
        public readonly int NumbersToGenerate;

        public RandomNumberRange(int minValueInclusive, int maxInclusiveValue)
        {
            if (minValueInclusive >= maxInclusiveValue)
                throw new ApplicationException("minimum number should be greater than maximum number in the range");

            this.Maximum = maxInclusiveValue;
            this.Minimum = minValueInclusive;
            this.NumbersToGenerate = this.Maximum - this.Minimum + 1;
        }
    }
}
```

You liked the code or was it helpful. Do let me know through your comments.

Happy Coding!!!!