---
title: "算法小汇"
date: 2017-05-11
categories:
  - algorithm
tags:
  - 算法
thumbnailImagePosition: left
thumbnailImage: //d1u9biwaxjngwg.cloudfront.net/highlighted-code-showcase/peak-140.jpg
---

Tranquilpeak Hugo theme have its own theme to highlight source code. It's based on GitHub theme: simple and elegant. Check out how it sublimate source codes.
<!--more-->

<!-- toc -->
# 三色旗

问题描述:一条绳子上悬挂了一组旗帜,旗帜分为三种颜色,现在需要把旗帜按顺序将相同的颜色的放在一起,没有旗帜的临时存放点,只能在绳子上操作,每次只能交换两个旗帜
例如:原本旗帜的顺序为rwbrbwwrbwbrbwrbrw需要变成bbbbbbwwwwwwrrrrrr
解决思路:遍历元素,如果元素该放到左边就与左边交换,该放到右边就与右边交换,左右边界动态调整,剩余的正好属于中间
{{< codeblock "sanseqi.java" "java"  >}}
public class Tricolour {

    private final static String leftColor = "b";
    private final static String rightColor = "r";

    public void tricolour(Object[] src){
        //中间区域的左端点
        int mli = 0;
        //中间区域的右端点
        int mri = src.length-1;
        //遍历元素,从mli开始到mri结束,mli与mri会动态调整,但是i比mli变化快
        for (int i = mli; i <= mri; i++) {
            //如果当前元素属于左边
            if(isPartOfLeft(src[i])){
                //将当前元素换与中间区域左端点元素互换
                swap(src,mli,i);
                //mli位的元素是经过处理的,一定不是红色,所以不需要再分析i位新元素
                //左端点右移
                mli++;
            }
            //如果当前元素属于右边
            else if(isPartOfRight(src[i])){
                //从中间区域的右端点开始向左扫描元素
                while (mri > i) {
                    //如果扫描到的元素属于右边,右端点左移,继续向左扫描,否则停止扫描
                    if(isPartOfRight(src[mri])){
                        mri--;
                    } else {
                        break;
                    }
                }
                //将当前元素交换到中间区域右端点
                swap(src,mri,i);
                //右端点左移
                mri--;
                //mri位的元素可能是蓝色的,需要再分析i位新元素
                i--;
            }
        }
    }

    private boolean isPartOfLeft(Object item){
        return leftColor.equals(item);
    }

    private boolean isPartOfRight(Object item){
        return rightColor.equals(item);
    }

    private void swap(Object[] src, int fst, int snd){
        Object tmp = src[fst];
        src[fst] = src[snd];
        src[snd] = tmp;
    }

    public static void main(String[] args) {
        String[] flags =
            new String[]{"r","b","w","w","b","r","r","w","b","b","r","w","b"};
        new Tricolour().tricolour(flags);
        for (String flag : flags) {
            System.out.printf("%s,",flag);
        }
    }
}
{{< /codeblock >}}

# 汉诺塔
{{<codeblock>}}
public class Hanoi {

    private void move(int n, char a, char b, char c){
        if(n == 1){
            System.out.printf("盘%d由%s移动到%s\n",n,a,c);
        } else{
            move(n-1,a,c,b);
            System.out.printf("盘%d由%s移动到%s\n",n,a,c);
            move(n-1,b,a,c);
        }
    }

    public static void main(String[] args) {
        new Hanoi().move(5, 'A','B','C');
    }
}
{{</codeblock>}}

# 斐波那契数列
{{<codeblock>}}
public class Fibonacci {

    private int[] src ;

    public int fibonacci(int n){
        if(src == null){
            src = new int[n+1];
        }
        if(n == 0 || n==1){
            src[n] = n;
            return n;
        }
        src[n] = fibonacci(n-1) + fibonacci(n-2);
        return src[n];
    }

    public int fibonacci2(int n){
        if(src == null){
            src = new int[n+1];
        }
        src[0] = 0;
        src[1] = 1;
        for (int i = 2; i < src.length; i++) {
            src[i] = src[i-1] + src[i-2];
        }
        return src[n];
    }

    public static void main(String[] args) {
        Fibonacci fib = new Fibonacci();
        int n = fib.fibonacci(20);
        System.out.println(n);
        for (int i : fib.src) {
            System.out.println(i);
        }
    }
}
{{</codeblock>}}

# 帕斯卡三角
{{<codeblock>}}
public class Pascal extends JFrame{

    private final int maxRow;

    Pascal(int maxRow){
        setBackground(Color.white);
        setTitle("帕斯卡三角");
        setSize(520, 350);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setVisible(true);
        this.maxRow = maxRow;
    }

    /**
     * 获取值
     * @param row 行号 从0开始
     * @param col 列号 从0开始
     * @return
     */
    private int value(int row,int col){
        int v = 1;
        for (int i = 1; i < col; i++) {
            v = v * (row - i + 1) / i;
        }
        return v;
    }

    @Override
    public void paint(Graphics g) {
        int r, c;
        for(r = 0; r <= maxRow; r++) {
            for(c = 0; c <= r; c++){
                g.drawString(String.valueOf(value(r, c)), (maxRow - r) * 20 + c * 40, r * 20 + 50);
            }
        }
    }

    public static void main(String[] args) {
        new Pascal(12);
    }
}
{{</codeblock>}}
