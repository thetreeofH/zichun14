---
title: 栈
date: 2020-04-06 16:24:05
tags: [数据结构, 算法, java, 栈]
description: 关于栈的学习笔记
category: [数据结构和算法]

---

# 1. 栈

> 代码仓库
>
> [Study: 一些资料 (gitee.com)](https://gitee.com/zichun14/Study)

## 1.1 介绍

- 栈的英文为(stack)
- 栈是一个**先入后出(FILO-First In Last Out)**的有序列表。
- 栈(stack)是限制线性表中元素的插入和删除只能在线性表的同一端进行的一种特殊线性表。允许插入和删除的一端，为**变化的一端，称为栈顶(Top)**，另一端为固定的一端，称为栈底(Bottom)。
- 根据栈的定义可知，最先放入栈中元素在栈底，最后放入的元素在栈顶，而删除元素刚好相反，最后放入的元素最先删除，最先放入的元素最后删除

## 1.2 应用场景

- 子程序的调用：在跳往子程序前，会先将下个指令的地址存到堆栈中，直到子程序执行完后再将地址取出，以回到原来的程序中。 	
- 处理递归调用：和子程序的调用类似，只是除了储存下一个指令的地址外，也将参数、区域变量等数据存入堆栈中。
- 表达式的转换[中缀表达式转后缀表达式]与求值(实际解决)。
- 二叉树的遍历。
- 图形的深度优先(depth一first)搜索法。

- 实现栈的思路分析
  - 使用数组模拟栈
  - 定义一个top表示栈顶，初始化为-1
  - 入栈操作，当有数据加入栈时，top++，stack[top]=val;
  - 出栈，int val = stack[top]，top--，return val;

## 1.3 代码示例（数组模拟栈）

```java
public class ArrayStackDemo {
    public static void main(String[] args) {
        ArrayStack stack = new ArrayStack(4);
        String key = "";
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop) {
            System.out.println("s(show): 显示栈");
            System.out.println("e(exit): 退出程序");
            System.out.println("push: 添加数据到栈顶");
            System.out.println("pop: 从栈中获取数据");
            key = scanner.next();
            switch (key) {
                case "s":
                    stack.list();
                    break;
                case "e":
                    scanner.close();
                    loop = false;
                    System.out.println("程序正在退出中");
                    break;
                case "push":
                    System.out.println("请输入要添加的数据");
                    int value = scanner.nextInt();
                    stack.push(value);
                    break;
                case "pop":
                    try {
                        int res = stack.pop();
                        System.out.printf("取出的数据是%d\n", res);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
            }
        }
    }
}

class ArrayStack {
    private int maxSize;
    private int[] stack;
    private int top = -1;

    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[maxSize];
    }

    public boolean isFull() {
        return top == maxSize - 1;
    }

    public void push(int value) {
        //先判断栈是否满
        if (isFull()) {
            System.out.println("栈已满~~");
            return;
        }
        top++;
        stack[top] = value;
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public int pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空~~");
        }
        int val = stack[top];
        top--;
        return val;
    }

    public void list() {
        if (isEmpty()) {
            System.out.println("pus栈为空~~");
        } else {
            for (int i = 0; i <= top; i++) {
                System.out.printf("stack[%d]=%d\n", i, stack[i]);
            }
        }

    }
}
```

## 1.4 链表模拟栈

```java
public class LinkedListStackDemo {
    public static void main(String[] args) {
        LinkedListStack stack = new LinkedListStack();
        String key = "";
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop) {
            System.out.println("s(show): 显示栈");
            System.out.println("e(exit): 退出程序");
            System.out.println("push: 添加数据到栈顶");
            System.out.println("pop: 从栈中获取数据");
            key = scanner.next();
            switch (key) {
                case "s":
                    stack.list();
                    break;
                case "e":
                    scanner.close();
                    loop = false;
                    System.out.println("程序正在退出中");
                    break;
                case "push":
                    System.out.println("请输入要添加的数据");
                    int value = scanner.nextInt();
                    stack.push(value);
                    break;
                case "pop":
                    try {
                        int res = stack.pop();
                        System.out.printf("取出的数据是%d\n", res);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
            }
        }
    }
}

class LinkedListStack {
    Node head = new Node(0);
    Node temp = head;

    public boolean isEmpty() {
        return head.next == null;
    }

    //入栈
    public void push(int no) {
        temp.next = new Node(no);
        temp = temp.next;
    }

    public int pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空~~");
        }
        Node pre = head;
        while (true) {
            if (pre.next == temp) {
                break;
            }
            pre = pre.next;
        }
        pre.next = temp.next;
        int val = temp.no;
        temp = pre;
        return val;

    }

    public void list() {
        Node temp = head;
        if (isEmpty()) {
            System.out.println("栈为空~~");
            return;
        }
        while (true) {
            if (temp.next == null) {
                break;
            }
            System.out.println(temp.next.no);
            temp = temp.next;
        }
    }
}

class Node {
    public int no;
    public Node next;

    public Node(int no) {
        this.no = no;
    }

    @Override
    public String toString() {
        return "Node{" +
                "no=" + no +
                ", next=" + next +
                '}';
    }
}
```

## 1.5 栈实现综合计算器（中缀表达式）

- 思路分析
  - 通过一个 index  值（索引），来遍历我们的表达式
  - 如果我们发现是一个数字, 就直接入数栈
  - 如果发现扫描到是一个符号,  就分如下情况
    - 如果发现当前的符号栈为 空，就直接入栈
    - 如果符号栈有操作符，就进行比较
      - 如果当前的操作符的优先级小于或者等于栈中的操作符，就需要从数栈中pop出两个数,在从符号栈中pop出一符号，进行运算，将得到结果，入数栈，然后将当前的操作符入符号栈
      - 如果当前的操作符的优先级大于栈中的操作符， 就直接入符号栈.
  - 当表达式扫描完毕，就顺序的从 数栈和符号栈中pop出相应的数和符号，并运行.
  - 最后在数栈只有一个数字，就是表达式的结果

- 代码实现

```java
public class Calculator {

    public static void main(String[] args) {
        //完成表达式计算
        String expression = "70+2*6-2";
        ArrayStack2 numStack = new ArrayStack2(10);
        ArrayStack2 operStack = new ArrayStack2(10);
        int index = 0;
        int num1 = 0;
        int num2 = 0;
        int oper = 0;
        int res = 0;
        char ch = ' ';
        String keepNum = "";
        while (true) {
            ch = expression.substring(index, index + 1).charAt(0);
            if (operStack.isOper(ch)) {
                if (operStack.isEmpty()) {
                    operStack.push(ch);
                } else {
                    if (operStack.priorty(ch) <= operStack.priorty(operStack.peek())) {
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1, num2, (char) oper);
                        numStack.push(res);
                    }
                    operStack.push(ch);
                }
            } else {
                //处理多位数
                //numStack.push(ch-48);
                keepNum += ch;

                //如果ch已经是最后一个
                if (index == expression.length() - 1) {
                    numStack.push(Integer.parseInt(keepNum));
                } else {

                    if (operStack.isOper(expression.substring(index + 1, index + 2).charAt(0))) {
                        numStack.push(Integer.parseInt(keepNum));
                        keepNum ="";
                    }
                }


            }
            index++;
            if (index >= expression.length()) {
                break;
            }
        }
        while (true) {
            if (operStack.isEmpty()) {
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1, num2, (char) oper);
            numStack.push(res);
        }
        System.out.printf("表达式%s = %d\n", expression, numStack.pop());
    }
}


class ArrayStack2 {
    private int maxSize;
    private int[] stack;
    private int top = -1;

    public ArrayStack2(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[maxSize];
    }

    public boolean isFull() {
        return top == maxSize - 1;
    }

    public void push(int value) {
        //先判断栈是否满
        if (isFull()) {
            System.out.println("栈已满~~");
            return;
        }
        top++;
        stack[top] = value;
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public int peek() {
        return stack[top];
    }

    public int pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空~~");
        }
        int val = stack[top];
        top--;
        return val;
    }

    public void list() {
        if (isEmpty()) {
            System.out.println("pus栈为空~~");
        } else {
            for (int i = 0; i <= top; i++) {
                System.out.printf("stack[%d]=%d\n", i, stack[i]);
            }
        }

    }

    //返回运算符号的优先级，优先级使用数字表示
    public int priorty(int oper) {
        if (oper == '*' || oper == '/') {
            return 1;
        } else if (oper == '+' || oper == '-') {
            return 0;
        } else {
            return -1;
        }
    }

    //判断是否是运算符
    public boolean isOper(char val) {
        return val == '*' || val == '-' || val == '+' || val == '/';
    }

    //计算方法
    public int cal(int num1, int num2, char oper) {
        int res = 0;
        switch (oper) {
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num2 - num1;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num2 / num1;
                break;
        }
        return res;


    }
}
```

## 1.6 逆波兰计算器

```java
public class PolandNotation {
    public static void main(String[] args) {

        //(31+2)*(4-1) => 3 2 + 4 1 - *
        String suffixExpression = "31 2 + 4 1 - *";
        //1.先将"3 2 + 4 1 - *"放到arrayList中
        //2.将arrayList传递给一个方法，配合栈完成计算
        List<String> listString = getListString(suffixExpression);
        System.out.println(listString);
        int calculate = calculate(listString);
        System.out.println("计算结果是："+calculate);
    }

    //将一个后缀逆波兰表达式放入arrayList中
    public static List<String> getListString(String expression) {
        String[] split = expression.split(" ");
        return new ArrayList<String>(Arrays.asList(split));

    }

    //完成对逆波兰表达式的计算

    /**
     * 3 2 + 4 1 - *
     * 从左至右扫描，将3和4压入堆栈；
     * 遇到+运算符，因此弹出4和3（4为栈顶元素，3为次顶元素），计算出3+4的值，得7，再将7入栈；
     * 将5入栈；
     * 接下来是×运算符，因此弹出5和7，计算出7×5=35，将35入栈；
     * 将6入栈；
     * 最后是-运算符，计算出35-6的值，即29，由此得出最终结果
     */
    public static int calculate(List<String> list) {
        Stack<String> stack = new Stack<>();
        for (String item : list) {
            //使用正则表达式
            if (item.matches("\\d+")) {
                stack.push(item);
            } else {
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = cal(num1, num2, item);
                stack.push(res+"");
            }
        }
        return Integer.parseInt(stack.pop());
    }

    //计算方法
    public static int cal(int num1, int num2, String oper) {
        int res = 0;
        switch (oper) {
            case "+":
                res = num1 + num2;
                break;
            case "-":
                res = num1 - num2;
                break;
            case "*":
                res = num1 * num2;
                break;
            case "/":
                res = num1 / num2;
                break;
        }
        return res;
    }
}
```

## 1.7 逆波兰计算器完整版

```java
public class ReversePolishMultiCalc {

    /**
     * 匹配 + - * / ( ) 运算符
     */
    static final String SYMBOL = "\\+|-|\\*|/|\\(|\\)";

    static final String LEFT = "(";
    static final String RIGHT = ")";
    static final String ADD = "+";
    static final String MINUS= "-";
    static final String TIMES = "*";
    static final String DIVISION = "/";

    /**
     * 加減 + -
     */
    static final int LEVEL_01 = 1;
    /**
     * 乘除 * /
     */
    static final int LEVEL_02 = 2;

    /**
     * 括号
     */
    static final int LEVEL_HIGH = Integer.MAX_VALUE;


    static Stack<String> stack = new Stack<>();
    static List<String> data = Collections.synchronizedList(new ArrayList<String>());

    /**
     * 去除所有空白符
     * @param s
     * @return
     */
    public static String replaceAllBlank(String s ){
        // \\s+ 匹配任何空白字符，包括空格、制表符、换页符等等, 等价于[ \f\n\r\t\v]
        return s.replaceAll("\\s+","");
    }

    /**
     * 判断是不是数字 int double long float
     * @param s
     * @return
     */
    public static boolean isNumber(String s){
        Pattern pattern = Pattern.compile("^[-\\+]?[.\\d]*$");
        return pattern.matcher(s).matches();
    }

    /**
     * 判断是不是运算符
     * @param s
     * @return
     */
    public static boolean isSymbol(String s){
        return s.matches(SYMBOL);
    }

    /**
     * 匹配运算等级
     * @param s
     * @return
     */
    public static int calcLevel(String s){
        if("+".equals(s) || "-".equals(s)){
            return LEVEL_01;
        } else if("*".equals(s) || "/".equals(s)){
            return LEVEL_02;
        }
        return LEVEL_HIGH;
    }

    /**
     * 匹配
     * @param s
     * @throws Exception
     */
    public static List<String> doMatch (String s) throws Exception{
        if(s == null || "".equals(s.trim())) throw new RuntimeException("data is empty");
        if(!isNumber(s.charAt(0)+"")) throw new RuntimeException("data illeagle,start not with a number");

        s = replaceAllBlank(s);

        String each;
        int start = 0;

        for (int i = 0; i < s.length(); i++) {
            if(isSymbol(s.charAt(i)+"")){
                each = s.charAt(i)+"";
                //栈为空，(操作符，或者 操作符优先级大于栈顶优先级 && 操作符优先级不是( )的优先级 及是 ) 不能直接入栈
                if(stack.isEmpty() || LEFT.equals(each)
                        || ((calcLevel(each) > calcLevel(stack.peek())) && calcLevel(each) < LEVEL_HIGH)){
                    stack.push(each);
                }else if( !stack.isEmpty() && calcLevel(each) <= calcLevel(stack.peek())){
                    //栈非空，操作符优先级小于等于栈顶优先级时出栈入列，直到栈为空，或者遇到了(，最后操作符入栈
                    while (!stack.isEmpty() && calcLevel(each) <= calcLevel(stack.peek()) ){
                        if(calcLevel(stack.peek()) == LEVEL_HIGH){
                            break;
                        }
                        data.add(stack.pop());
                    }
                    stack.push(each);
                }else if(RIGHT.equals(each)){
                    // ) 操作符，依次出栈入列直到空栈或者遇到了第一个)操作符，此时)出栈
                    while (!stack.isEmpty() && LEVEL_HIGH >= calcLevel(stack.peek())){
                        if(LEVEL_HIGH == calcLevel(stack.peek())){
                            stack.pop();
                            break;
                        }
                        data.add(stack.pop());
                    }
                }
                start = i ;    //前一个运算符的位置
            }else if( i == s.length()-1 || isSymbol(s.charAt(i+1)+"") ){
                each = start == 0 ? s.substring(start,i+1) : s.substring(start+1,i+1);
                if(isNumber(each)) {
                    data.add(each);
                    continue;
                }
                throw new RuntimeException("data not match number");
            }
        }
        //如果栈里还有元素，此时元素需要依次出栈入列，可以想象栈里剩下栈顶为/，栈底为+，应该依次出栈入列，可以直接翻转整个stack 添加到队列
        Collections.reverse(stack);
        data.addAll(new ArrayList<>(stack));

        System.out.println(data);
        return data;
    }

    /**
     * 算出结果
     * @param list
     * @return
     */
    public static Double doCalc(List<String> list){
        Double d = 0d;
        if(list == null || list.isEmpty()){
            return null;
        }
        if (list.size() == 1){
            System.out.println(list);
            d = Double.valueOf(list.get(0));
            return d;
        }
        ArrayList<String> list1 = new ArrayList<>();
        for (int i = 0; i < list.size(); i++) {
            list1.add(list.get(i));
            if(isSymbol(list.get(i))){
                Double d1 = doTheMath(list.get(i - 2), list.get(i - 1), list.get(i));
                list1.remove(i);
                list1.remove(i-1);
                list1.set(i-2,d1+"");
                list1.addAll(list.subList(i+1,list.size()));
                break;
            }
        }
        doCalc(list1);
        return d;
    }

    /**
     * 运算
     * @param s1
     * @param s2
     * @param symbol
     * @return
     */
    public static Double doTheMath(String s1,String s2,String symbol){
        Double result ;
        switch (symbol){
            case ADD : result = Double.valueOf(s1) + Double.valueOf(s2); break;
            case MINUS : result = Double.valueOf(s1) - Double.valueOf(s2); break;
            case TIMES : result = Double.valueOf(s1) * Double.valueOf(s2); break;
            case DIVISION : result = Double.valueOf(s1) / Double.valueOf(s2); break;
            default : result = null;
        }
        return result;

    }

    public static void main(String[] args) {
        //String math = "9+(3-1)*3+10/2";
        String math = "12.8 + (2 - 3.55)*4+10/5.0";
        try {
            doCalc(doMatch(math));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}
```
