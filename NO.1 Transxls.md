#### 第1题：对表格P.relative.xls进行转置，得到结果文件：P.relative.tran.xls
+ excel的转置
#### 第2题：根据分组信息gmderoup.list 对 P.relative.xls 中的数据按照组来算平均值和标准差（输出值“avg：sd”），得到结果文件：goup.P.relative.xls
+ 两个表格之间的联系
 
 ---
 
 ### 程序规划
 1. 读入Excel数据P.relative.xls
 
 2. **Excel转置**
 
 > Excel转置的含义：行列互换。
 
 + 获取excel行列的值
 
 + 进行互换
 
 3. 输出到文件P.relative.tran.xls
 
 
 #### 基于哈希的算法
 ```
 #!/usr/bin/perl -w
 use strict;
 unless (@ARGV == 2 ){
  die "Usage perl $0<P.relative.xls><P.relative.tran.xls>";
}
my ($infile,$outfile) = @ARGV;
open IN,'<',$infile or die $!;
open OUT,'>',$outfile or die $!;
my $line_no=0;
my ($num,%hash);
while (<IN>) {
	chomp;
	next if (/^$/);#遇到空行就跳过
	$line_no++;
	my @lie=split (/\t/,$_);#把读取的行数据分割为数组，便于引用的赋值
	$num=@lie;
	for (my $i=0;$i<$num ;$i++) {
		$hash{$line_no}{$i}=$lie[$i];  #line_no 行数 i 列数 lie[i] 值 ； 哈希的赋值
	}
}

for (my $L=0;$L<$num ;$L++) {  #嵌套循环
	for (my $H=1;$H<=$line_no ;$H++) {
		
	print OUT $hash{$H}{$L},"\t";#行列互换
	}
	print OUT "\n";
}
close IN;
close OUT;
```

---

#### 另一种基于数组的算法
 
```
#!/usr/bin/perl -w
 use strict;
 unless (@ARGV == 2 ){
  die "Usage perl $0<P.relative.xls><P.relative.tran.xls>";
}
my ($infile,$outfile) = @ARGV;
open IN,'<',$infile or die $!;
open OUt,'>',$outfile or die $!;
my $line_no=0;
my ($num,@arr);
while (<IN>) {
	chomp;
	next if (/^$/);#遇到空行就跳过
	$line_no++;
	my @lie=split (/\t/,$_);#把读取的行数据分割为数组，便于引用的赋值
	$num=@lie;
	for (my $i=0;$i<$num ;$i++) {
		@{$arr[$line_no]}[$i]=$lie[$i];  #line_no 行数 i 列数 lie[i] 值 ； 嵌套数组的赋值
	}
}

for (my $L=0;$L<$num ;$L++) {  #嵌套循环
	for (my $H=1;$H<=$line_no ;$H++) {
		
	print OUT @{$arr[$H][$L],"\t";#行列互换
	}
	print OUT "\n";
}
close IN;
close OUT;
```
---

### 第二题
 
#### 流程规划
```
1. 用分组信息gmderoup.list 将分组信息存入数组/哈希中   #数组还是哈希你要选择一种 #用数组吧
unless(@ARGV ==3 ){
	die "Usage perl $0,<gmderoup.list>,<P.relative.xls>,<goup.P.relative.xls>";
	}
my ($infile1,$infile2,$outfile) = @ARGV;
open IN1,'<',$infile1,or die $!;
open IN2,'<',$infile2,or die $!;
open OUT,'>',$outfile,or die $!;
#################
#分组信息如何确定？把编号放到分组中
################
my $line_no=0;
while (IN1){
	chomp;
	my @arr = split /\t/ $_;
	$line_no++;
	#######################################
	#换为下一组，当$line_no是某个数的倍数时
	#怎样构建一个分组循环？有必要吗？
	#######################################
	
	
2. 用P.relative.xls将其中的元素分组放在上一步数组/哈希中
	+ 嵌套结构的构建?
	+ grep ?
	+ cat ?
	+ split ?
	+ awk ?
	
3. 计算每一组的aver与sd,写一个子程序，输入数组，输出aver与sd
sub aver {
	
	my(@ref)= @_;
	my $sum = 0;
	$sum = grep { $sum += $_} @erf;
	my $aver = $sum/ @erf;
	return $aver;
}

sub sd {
    my $arr = shift;
    my $v = aver($arr);
    my $d = 0;
    grep {$d += ($_-$v)**2;}@$arr;
	$d = ($d/@$arr)**(1/2);
    return $d;
}
	
4. 格式的转化
5. 输出 每一组的计算结果

```
