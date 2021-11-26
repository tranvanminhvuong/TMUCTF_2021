# TMUCTF-2021
# 1/ UnBioSed

- Ở phân packer ta thấy có PyInstaller⇒ phải unpack nó

https://user-images.githubusercontent.com/89820487/143584832-86ff9ce4-4d29-4f9b-b927-d11974a5eaf8.png

```clojure
1/ convert .exe => .pyc 
#exe to pyc 
https://github.com/extremecoders-re/pyinstxtractor
2/ convert .pyc => .py
#pyc to py
* Sử dụng uncompyle6 (https://pypi.org/project/uncompyle6/) (python 3.3 ~ python 3.6)
* decompile to py: Uncompyle6 <file.pyc>
- Nếu decompile báo lỗi: thử decompile một file khác, nếu thành công copy 8 byte đầu tiên của file đó paste đè vào 8 byte đầu của <file.pyc>, sau đó decompile again
```

- Git clone  https://github.com/extremecoders-re/pyinstxtractor
- Trên Terminal :`python pyinstxtractor.py <filename>`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b19c6396-6170-4ceb-ba27-e283f242f520/Untitled.png)

- Dowload : uncompyle6

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c50b8e15-a5fd-4f61-8542-c0e24b438ab9/Untitled.png)

- So sánh input vs h[] .Nếu bằng ⇒ Có Flag

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8354d3ac-1cfb-4414-bd10-9f93b522339e/Untitled.png)

⇒Flag: TMUCTF{M4y_4ll_Y0ur_D3c1510n5_b3_Unb1053d!}

# 2/Print_Flag

- Run file ta thấy flag được in ra và càng về sau càng lâu.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/82c78ec4-ed1f-4069-bcbd-12e26bed7a3d/Untitled.png)

- Mở file bằng Gidra

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/57253245-d631-497d-9dbc-8728f7899989/Untitled.png)

- Sau khi đọc qua code thấy được có hàm sleep() sau đó in ra ký tự (Chính là flag). Tôi đã chạy và pypass nhưng vẫn k giải quyết được vấn đề.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c645858-5e4f-4f19-884c-d0018d7e1ea9/Untitled.png)

- Kiểm tra hàm FUN_00101189()

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3737024f-8b76-4fba-9d0d-e8a3ac538b13/Untitled.png)

- Thấy ngay 2 hàm đệ quy ⇒ vấn đề nằm ở đây
- Phải code lại thôi
- Sử dụng Dynamic Programing

```cpp
#include <iostream>
using namespace std;
int arr[1000];
void init_Dequy()
{
	for(int i=0;i<1000;++i)
		arr[i]=0;
}
int FUN_00101189(int param_1);
int dequy(int k);
int map[]={ 0x2e, 0x00, 0x00, 0x00, 0x22, 0x00, 0x00, 0x00, 0x83, 0x09, 0x00, 0x00, 0x11, 0xce, 0x0b, 0x00, 0x12, 0x66, 0x0b, 0x00, 0x20, 0xfd, 0x09, 0x00, 0x7f, 0x00, 0x00, 0x00, 0x23, 0xdc, 0x07, 0x00, 0x72, 0x13, 0x03, 0x00, 0x45, 0x00, 0x00, 0x00, 0xe5, 0x15, 0x0a, 0x00, 0x85, 0x39, 0x01, 0x00, 0x1a, 0x43, 0x09, 0x00, 0xb2, 0x00, 0x00, 0x00, 0xa6, 0x00, 0x00, 0x00, 0x9b, 0x92, 0x0d, 0x00, 0xc9, 0xe5, 0x08, 0x00, 0xba, 0xb1, 0x04, 0x00, 0x69, 0xe1, 0x0d, 0x00, 0x68, 0x92, 0x01, 0x00, 0x55, 0xcf, 0x0b, 0x00, 0x8f, 0xc5, 0x06, 0x00, 0x31, 0x01, 0x03, 0x00, 0x86, 0xec, 0x06, 0x00, 0xe5, 0x39, 0x06, 0x00, 0x6f, 0x30, 0x0c, 0x00, 0xc0, 0x02, 0x00, 0x00, 0xf3, 0x02, 0x00, 0x00, 0x1d, 0x8b, 0x07, 0x00, 0x3f, 0x43, 0x02, 0x00, 0x34, 0x20, 0x0d, 0x00, 0xe8, 0x03, 0x00, 0x00, 0xe1, 0x15, 0x03, 0x00, 0x63, 0x21, 0x0c, 0x00, 0xf5, 0x04, 0x00, 0x00, 0x35, 0xec, 0x01, 0x00, 0x36, 0x37, 0x00, 0x00, 0x25, 0x05, 0x00, 0x00 };
int main()
{
	int local_2c=4,uVar1=62,bVar2;
	for (local_2c = 4; local_2c < 38; local_2c = local_2c + 1) {
	    uVar1 = map[local_2c*4];
	    bVar2 = FUN_00101189(local_2c*local_2c);
	    putchar((int)(char)(((uVar1 ^ bVar2) - (char)local_2c) + '&'));
  	}
	return (0);
}
int FUN_00101189(int param_1)
{
  int iVar1;
  int iVar2;
  unsigned int local_20;
  unsigned int local_1c;
  
  if (param_1 == 0) {
    iVar1 = 0;
  }
  else {
    if (param_1 == 1) {
      iVar1 = 10;
    }
    else {
      local_20 = 0;
      for (local_1c = 1; (int)local_1c < param_1; local_1c = local_1c + 1) {
        local_20 = (int)((local_1c ^ local_20) + param_1) % 10;
      }
      iVar1 = dequy(param_1 + -1);
      iVar2 = dequy(param_1 + -2);
      iVar1 = (int)(iVar1 * local_20 + iVar2 * local_20 + param_1) % 1000000;
    }
  }
  return iVar1;
}
int dequy(int k)
{
	int res;
	if(arr[k]==0)
	{
		res=FUN_00101189(k);
		arr[k]=res;
	}
	else
		res=arr[k];
	return res;
}
```

- Run file có đc flag
- TF{7h15_l4zy_f1l3_5l33p5_700_much}
- ⇒ Flag: TMUCTF{7h15_l4zy_f1l3_5l33p5_700_much}
