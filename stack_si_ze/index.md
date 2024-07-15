# 栈应用之四则运算

## 四则运算
四则运算表达式是我们小学就接触的内容，它遵循“先乘除，后加减，从左到右，括号内先算”的法则，例如“7 + (5 - 3) * 4 + 6 / 3”，这个表达式先算**5 - 3**得**2**，再算**2 \* 4**和**6 / 3**，最后计算**7 + 8 +2**，这个过程很简单，口算就能完成，但是如果让我们在程序里实现这个功能，该如何实现呢？我们遇到的困难在于乘除优在加减的后面，却要先运算，另外还要考虑括号，问题就复杂了。

<!-- more -->

## 后缀表达式
波兰有位科学家也想到了这个问题，他想出了一种新的**不需要括号**的表达式：“后缀表达式”，它更有利于计算机计算。让我们看看它的样子：对于“7 + (5 - 3) * 4 + 6 / 3”，后缀表达式为“7 5 3 - 4 * + 6 3 / +”，叫后缀的原因在于**运算符在操作数之后**。我们人类喜欢看到的表达式叫“中缀表达式”（因为运算符在操作数中间），但是计算机不喜欢它。

### 后缀表达式的方法
为了看到后缀表达式的好处，我们先看看，计算机如何利用后缀表达式计算出最终结果。
* 后缀表达式：**7 5 3 - 4 \* + 6 3 / +**
* 规则：从左到右遍历字符串，遇到数字则进栈，遇到符号则将栈顶的两个数字出栈，进行计算，运算结果进栈，一直到最终获得结果。

1. 初始化一个空栈，此栈用来对要运算的数字进出使用。
2. 字符串中前三个都是数字，所以7，5，3进栈。

![过程](https://s2.ax1x.com/2019/08/20/mGkVZ6.jpg)


3. 接下来是“-”，所以5和3出栈，5作为被减数，3作为减数，5减3得到2，并将2入栈。
4. 接着是4入栈。

![过程](https://s2.ax1x.com/2019/08/20/mGkByn.jpg)

5. 接下来是”\*“，所以4和2出栈，4乘以2得8，8入栈。
6. 下面是“+”，7和8出栈，7加8得15，15入栈。

![过程](https://s2.ax1x.com/2019/08/20/mGk7TK.jpg)

7. 接下来6和3数字入栈。
8. 遇到符号“/”，所以6和3出栈，6作为被除数，3作为除数，6除3得2，2入栈。

![过程](https://s2.ax1x.com/2019/08/20/mGA9Tf.jpg)

9. 最后遇到符号“+”，15和2出栈，15加2得17，17入栈，遍历结束，将最后结果出栈，得到17。

![过程](https://s2.ax1x.com/2019/08/20/mGAV6s.jpg)


### 中缀表达式转后缀表达式
可以看到利用**栈**就很容易计算后缀表达式的值，那么现在我们的问题就是中缀转后缀。

* 中缀表达式：“8 + (7 - 2 * 3 + 2) * 3 + 10 / 2”
* 规则：遍历字符串，遇到数字则输出，即成为后缀表达式一部分；若是操作符，则判断与栈顶符号的优先级（乘除优先级比加减优先级高，乘除优先级一样，加和减也一样），如果高于栈顶符号，则压栈，否则从栈顶开始弹出元素直到遇到遇到优先级更低的符号（或者遇到“(”，“(”只有遇到“)”才会弹出），弹出完这些符号后，把当前符号压栈。


1. 初始化一空栈，用来对符号进出栈使用。
2. 第一个字符是数字8，输出8，后面符号是“+”，进栈。

![过程](https://s2.ax1x.com/2019/08/21/mUD2S1.png)

3. 第三个字符是“(”，因为是左括号，所以压栈，第四个字符是7，输出，总表达式为8 7。
4. 接着是“-”，因为栈顶是“(”，所以压栈。后面字符是2，输出，总表达式为8 7 2。

![过程](https://s2.ax1x.com/2019/08/25/mgNyRO.png)

5. 之后符号是“\*”，它的优先级比栈顶“-”高，所以压栈，再之后是数字3，输出，总表达式为8 7 2 3。
6. 接着是符号“+”，它比“\*”的优先级低，所以“\*”弹出栈输出，而“-”优先级和“+”一样，也要弹出栈输出，接下来碰到符号“(”，就要把“+”压栈。接着是数字2，输出，总表达式为8 7 2 3 \* - 2。

![过程](https://s2.ax1x.com/2019/08/25/mgUPlF.png)

7. 接着是符号“)”，这时需要从栈顶开始依次弹出符号输出，直到遇到“(”（“(”也要弹出，只是不输出），“(”之后只剩一个“+”，所以弹出“+”输出，接下来是符号“\*”，优先级比“+”高，所以压栈，总表达式为8 7 2 3 \* - 2 +。
8. 接下来是数字3，输出，紧接着是符号“+”，它比栈顶“\*”优先级低，所以弹出“\*”输出，而之后比较的“+”优先级一样，也弹出栈输出，最后“+”压栈，总表达式为8 7 2 3 \* - 2 + 3 \* +。

![过程](https://s2.ax1x.com/2019/08/28/moOHG6.jpg)

9. 接着是数字10，输出，接下来是符号“/”，比符号“+”优先级高，所以压栈，总表达式为8 7 2 3 \* - 2 + 3 \* + 10。
10. 接着是数字2，输出。遍历结束，依次弹出栈中元素，最后总表达式为8 7 2 3 \* - 2 + 3 \* + 10 2 / +。

![过程](https://s2.ax1x.com/2019/08/28/moXELQ.jpg)

### 代码示例
```
// 预先生成运算符的tokens
prepareTokens() {
    this.tokens = [
        new Token('#', TOKEN_TYPE.ENDEXPR),
        new Token('(', TOKEN_TYPE.LEFTPAREN),
        new Token(')', TOKEN_TYPE.RIGHTPAREN),
        new Token('~', TOKEN_TYPE.UNARYOP, 6),       // 负号
        new Token('abs', TOKEN_TYPE.UNARYOP, 6),     // 求绝对值
        new Token('sqrt', TOKEN_TYPE.UNARYOP, 6),    // 开平方根
        new Token('exp', TOKEN_TYPE.UNARYOP, 6),     // e的x次
        new Token('ln', TOKEN_TYPE.UNARYOP, 6),      // e为底数的对数
        new Token('log10', TOKEN_TYPE.UNARYOP, 6),   // 10为底数的对数
        new Token('sin', TOKEN_TYPE.UNARYOP, 6),     // 求sin x
        new Token('cos', TOKEN_TYPE.UNARYOP, 6),     // 求cos x
        new Token('tan', TOKEN_TYPE.UNARYOP, 6),     // 求tan x
        new Token('+', TOKEN_TYPE.BINARYOP, 4),      // 二元+
        new Token('-', TOKEN_TYPE.BINARYOP, 4),      // 二元-
        new Token('*', TOKEN_TYPE.BINARYOP, 5),      // 乘法
        new Token('/', TOKEN_TYPE.BINARYOP, 5),      // 除法
        new Token('%', TOKEN_TYPE.BINARYOP, 5),      // 除模取余
        new Token('^', TOKEN_TYPE.BINARYOP, 6),      // 指数运算
    ]
}

/**
 * 中缀表达式转化为后缀表达式
 * @return {Array}
 */
transform() {
    const postExp = []
    const opStack = []
    for (let i = 0; i < this.infixExp.length; i++) {
        const pos = this.infixExp[i]
        const token = this.tokens[pos]
        switch (token.type) {
            case TOKEN_TYPE.OPRAND:
                postExp.push(pos)
                break;
            case TOKEN_TYPE.LEFTPAREN:  // “(”直接入栈
                opStack.push(pos)
                break;
            case TOKEN_TYPE.RIGHTPAREN: // 为“)”，出栈直到遇到运算符“(”
                let prePos = opStack.pop()
                while (prePos in this.tokens && opStack.length >= 0 &&
                this.tokens[prePos].type !== TOKEN_TYPE.LEFTPAREN) {
                    postExp.push(prePos)
                    prePos = opStack.pop()
                }
                break;
            case TOKEN_TYPE.UNARYOP:
            case TOKEN_TYPE.BINARYOP:
                let endright = 0
                while (endright === 0) {
                    if (opStack.length <= 0)
                        endright = 1
                    else if (this.tokens[opStack[opStack.length - 1]].type === TOKEN_TYPE.LEFTPAREN) {
                        endright = 1
                    } else if (this.tokens[opStack[opStack.length - 1]].priority < token.priority) {
                        endright = 1
                    } else if (this.tokens[opStack[opStack.length - 1]].priority === token.priority &&
                                token.priority === MAX_PRIORITY) {
                        endright = 1
                    } else {
                        postExp.push(opStack.pop())
                        endright = 0
                    }
                }
                opStack.push(pos)
                break
            case TOKEN_TYPE.ENDEXPR:
                while (opStack.length >= 1) {
                    postExp.push(opStack.pop())
                }
                break
            default:
                break
        }
    }

    postExp.push(0)  // 添加终止符
    return postExp
}

```
中缀表达式`infixExp`中存的是`this.tokens`中的索引，完整代码[Github](https://github.com/salamander-mh/calculator)
