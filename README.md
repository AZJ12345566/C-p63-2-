# C-p63-2-
C语言学习笔记 p63 动态内存分配(2)
#include<stdio.h>
#include<stdlib.h>
#include<errno.h>
//calloc会将所有元素初始化为0
int main()
{
    int* p=(int*)calloc(10,sizeof(int));
    if(p==NULL)
    {
        printf("%s\n",strerror(errno));
    }
    else
    {
        int i=0;
        for(i=0;i<10;i++)
        {
            printf("%d",*(p+i));
        }
    }
    //释放空间
    free(p);
    p=NULL;
    return 0;
}

//realloc可以调整动态开辟内存的空间大小
//realloc库函数模版：void* realloc(void* memblock,size_t size);
int main()
{
    int* p=(int*)malloc(20);
    if(p==NULL)
    {
        printf("%s\n",strerror(errno));
    }
    else
    {
        int i=0;
        for(i=0;i<5;i++)
        {
            *(p+i)=i;
        }
    }
    //只是在使用malloc开辟的20个字节空间
    //假设这里20个不能满足使用要求
    //假设这里需要40个字节
    //这里就可以使用realloc来调整
    //realloc使用的注意事项：
    //1.如果p指向的空间之后有足够的内存空间可以追加，则直接追加，后返回p
    //2.如果p指向的空间之后没有足够的内存空间可以追加，则realloc函数会重新找一块新的内存区域开辟一块满足需求的内存空间，并且把原来内存中的数据拷贝回来，释放旧的内存空间，之后返回新开辟内存空间的地址
    //3.得用一个新的变量来接收realloc函数的返回值
    int* ptr=realloc(p,40);
    if(ptr!=NULL)
    {
        p=ptr;
        int i=0;
        for(i=5;i<10;i++)
        {
            *(p2+i)=i;
        }
        for(i=0;i<10;i++)
        {
            printf("%d ",*(p2+i));
        }
    }
    //释放内存
    free(p);
    p=NULL;
    return 0;
}

int main()
{
    int* p=(int*)malloc(40);
    //1.对NULL进行解引用操作
    //万一malloc失败了，p就被赋值为NULL
    //所以就必须进行if语句的判断
    if(p==NULL)
    {
        printf("%s\n",strerror(errno));
    }
    int i=0;
    for(i=0;i<10;i++)
    {
        *(p+i)=i;
    }
    free(p);
    p=NULL;
    return 0;
}

int main()
{
    //2.对动态开辟内存的越界访问
    int* p=(int*)malloc(5*sizeof(int));
    if(p==NULL)
    {
        return 0;
    }
    else
    {
        int i=0;
        for(i=0;i<10;i++)
        {
            *(p+i)=i;
        }
    }
    free(p);
    p=NULL;
    return 0;
}

int main()
{
    int a=10;//这是栈区的内存空间
    int* p=&a;
    *p=20;
    //3.对非动态内存的释放
    free(p);
    p=NULL;
    return 0;
}
