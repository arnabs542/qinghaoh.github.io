---
layout: post
title:  "Expression Evaluation"
---
## Basic

|    Infix    |  Prefix   |  Postfix  |
|-----------------------------------|
|    a + b    |   + a b   |   a b +   |
|  a + b * c  | + a * b c | a b c * + |
| (a + b) * c | * + a b c | a b + c * |

## Expression Tree

[Build Binary Expression Tree From Infix Expression][build-binary-expression-tree-from-infix-expression]

{% highlight java %}
public Node expTree(String s) {
    // converts infix to postfix
    StringBuilder postfix = new StringBuilder();
    Deque<Character> opstack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (Character.isDigit(c)) {
            postfix.append(c);
        } else {
            if (c == '(') {
                opstack.push(c);
            } else if (c == ')') {
                while (opstack.peek() != '(') {
                    postfix.append(opstack.pop());
                }
                opstack.pop();
            } else if (c == '+' || c == '-') {
                while (!opstack.isEmpty() && opstack.peek() != '(') {
                    postfix.append(opstack.pop());
                }
                opstack.push(c);
            } else {
                while (!opstack.isEmpty() && (opstack.peek() == '*' || opstack.peek() == '/')) {
                    postfix.append(opstack.pop());
                }
                opstack.push(c);
            }
        }
    }

    while (!opstack.isEmpty()) {
        postfix.append(opstack.pop());
    }

    // builds inorder expression tree from postfix
    Deque<Node> operandStack = new ArrayDeque<>();
    for (char c : postfix.toString().toCharArray()) {
        if (Character.isDigit(c)) {
            operandStack.push(new Node(c));
        } else {
            Node right = operandStack.pop(), left = operandStack.pop();
            operandStack.push(new Node(c, left, right)); 
        }
    }
    return operandStack.pop();
}
{% endhighlight %}

[Basic Calculator III][basic-calculator-iii]

{% highlight java %}
private static final Map<Character, Integer> PRECEDENCE = new HashMap<>();

static {
    PRECEDENCE.put('(', -1);
    PRECEDENCE.put('+', 0);
    PRECEDENCE.put('-', 0);
    PRECEDENCE.put('*', 1);
    PRECEDENCE.put('/', 1);
}

public int calculate(String s) {
    // infix -> postfix
    Deque<Integer> operands = new ArrayDeque<>();
    Deque<Character> operators = new ArrayDeque<>();
    int n = s.length();
    for (int i = 0; i < n; i++) {
        char c = s.charAt(i);
        if (Character.isDigit(c)) {
            int val = Character.getNumericValue(s.charAt(i));
            while (i + 1 < n && Character.isDigit(s.charAt(i + 1))) {
                val = val * 10 + Character.getNumericValue(s.charAt(i + 1));
                i++;
            }
            operands.push(val);
        } else if (c == '(') {
            operators.push(c);
        } else if (c == ')') {
            while (operators.peek() != '(') {
                operands.push(operate(operands, operators));
            }
            operators.pop();
        } else {
            while (!operators.isEmpty() && compare(c, operators.peek()) <= 0) {
                operands.push(operate(operands, operators));
            }
            operators.push(c);
        }
    }

    while (!operators.isEmpty()) {
        operands.push(operate(operands, operators));
    }

    return operands.pop();
}

// precedence
private int compare(char a, char b) {
    return PRECEDENCE.get(a) - PRECEDENCE.get(b);
}

private int operate(Deque<Integer> operands, Deque<Character> operators) {
    int a = operands.pop(), b = operands.pop();
    char c = operators.pop();

    switch(c) {
        case '+' : return a + b;
        case '-': return b - a;
        case '*': return a * b;
        case '/': return b / a;
        default: return 0;
    }
}
{% endhighlight %}

[basic-calculator-iii]: https://leetcode.com/problems/basic-calculator-iii/
[build-binary-expression-tree-from-infix-expression]: https://leetcode.com/problems/build-binary-expression-tree-from-infix-expression/
