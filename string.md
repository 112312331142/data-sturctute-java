## 九、字符串

### 9.1 二进制求和

给你两个二进制字符串 `a` 和 `b` ，以二进制字符串的形式返回它们的和。

#### 1

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        int i1 = a.size() - 1, i2 = b.size() - 1;
        int carry = 0;//妙
        //用carry/2来表示上一位是否有进位，只有当carry>2(carry为2或者3时，才会进位)
        string ans;
        while (i1 >= 0 && i2 >= 0) {
            int sum = carry;//sum表示这一位相加后的数值
            sum += (a.at(i1--) - '0') + (b.at(i2--) - '0');//相加求和
            carry = sum / 2;//进位
            ans += ('0' + sum % 2);//'0'+1来将int类型的数字转换为字符串类型
        }
        while (i1 >= 0) {//i1与i2长度不相等时，会有一个有剩余
            int sum = carry + a.at(i1--) - '0';
            carry = sum / 2;
            ans += ('0' + sum % 2);
        }
        while (i2 >= 0) {
            int sum = carry + b.at(i2--) - '0';
            carry = sum / 2;
            ans += ('0' + sum % 2);
        }
        if (carry == 1) {
            ans += '1';//最后如果还有进位，字符串要在加一
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

