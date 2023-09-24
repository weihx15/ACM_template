## 输入

```Java
class InputReader {
	BufferedReader buf;
	StringTokenizer tok;
	InputReader() {
		buf = new BufferedReader(new InputStreamReader(System.in));
	}
	boolean hasNext() {
		while (tok == null || !tok.hasMoreElements()) {
			try {
				tok = new StringTokenizer(buf.readLine());
			} catch (Exception e) {
				return false;
			}
		}
		return true;
	}
	String next() {
		if (hasNext())
			return tok.nextToken();
		return null;
	}
	int nextInt() {
		return Integer.parseInt(next());
	}
	long nextLong() {
		return Long.parseLong(next());
	}
	double nextDouble() {
		return Double.parseDouble(next());
	}
	BigInteger nextBigInteger() {
		return new BigInteger(next());
	}
	BigDecimal nextBigDecimal() {
		return new BigDecimal(next());
	}
}
```

## 输出

```Java
System.out.println(n); //基本的输出方法
PrintWriter out = new PrintWriter(new BufferedOutputStream(System.out));//使用缓存加速，比直接使用System.out快
out.println(n); 
out.printf("%.2f\n", ans); // 与c语言中printf用法相同
```

## 进制转换

```Java
String s = Integer.toString(a, x); //把int型数据转换乘X进制数并转换成string型
int b = Integer.parseInt(s, x);//把字符串当作X进制数转换成int型
```

## BigInteger 类

```Java
BigInteger bi = new BigInteger("5");
```

|方法名称|说明|
|---|---|
|add(BigInteger val)|做加法运算|
|subtract(BigInteger val)|做减法运算|
|multiply(BigInteger val)|做乘法运算|
|divide(BigInteger val)|做除法运算|
|remainder(BigInteger val)|做取余数运算|
|divideAndRemainder(BigInteger val)|做除法运算，返回数组的第一个值为商，第二个值为余数|
|pow(int exponent)|做参数的 exponent 次方运算|
|negate()|取相反数|
|shiftLeft(int n)|将数字左移 n 位，如果 n 为负数，则做右移操作|
|shiftRight(int n)|将数字右移 n 位，如果 n 为负数，则做左移操作|
|and(BigInteger val)|做与运算|
|or(BigInteger val)|做或运算|
|compareTo(BigInteger val)|做数字的比较运算|
|equals(Object obj)|当参数 obj 是 Biglnteger 类型的数字并且数值相等时返回 true, 其他返回 false|
|min(BigInteger val)|返回较小的数值|
|max(BigInteger val)|返回较大的数值|

## BigDecimal 类

- BigDecimal(double val)：实例化时将双精度型转换为 BigDecimal 类型。
- BigDecimal(String val)：实例化时将字符串形式转换为 BigDecimal 类型。

```Java
BigDecimal add(BigDecimal augend)    // 加法操作
BigDecimal subtract(BigDecimal subtrahend)    // 减法操作
BigDecimal multiply(BigDecimal multiplieand)    // 乘法操作
BigDecimal divide(BigDecimal divisor,int scale,int roundingMode )    // 除法操作
```

## 正则表达式

```Java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {
        String regex = "[a-z]@.";
        Pattern p = Pattern.compile(regex);
        String str = "abc@yahoo.com,123@cnn.com,abc@google.com";
        Matcher m = p.matcher(str);

        if (m.find()) {
            String foundStr = str.substring(m.start(), m.end());
            System.out.println("Found string  is:" + foundStr);
        }
    }
}
```

