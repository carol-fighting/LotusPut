----------
# ���������ݽṹ


 - ��������ܴ󣬲���ȫ�����ڴ棬�������ļ�����ʽ���ڴ���
 - ���ҹ��̲�������IO���ģ��������ٴ���IO�Ĵ�ȡ����
	 -  B�����壬����һ��������h���ڵ㣬O(logdN)���ݣ�d����100,h���Ϊ3
	 - �ڵ�Ĵ�С��Ϊҳ��С��ֻҪһ��IO�Ϳ�����ȫ����
	 

----------
# B����B+ ��
 - B����

    - ÿ���ڵ㶼�ǹؼ���

 - B+����

    - ��Ҷ�ڵ�������������ã��ؼ���ֻ�����Ҷ�ڵ�

    - Ҷ�ڵ㹹�����������ɰ��ؼ�������Ĵ���������м�¼

----------
# ��������
 - ������**�洢����**ʵ�֣����������ݽṹ
 - ��ͨ������create index �������� on ���� (�ֶ�(����));
 - Ψһ�����������е�ֵ����Ψһ��create unique index 

 - ���������������ͬʱ��Ĭ�����ɵ�
 - ȫ��������full text����������myisam��char��varchar��text���ϴ���
 - ��������
 - ����ǰ׺

----------
# �洢����
 
- myisam���Ǿۼ�����
	- �ڴ��̴������ļ�,�洢���塢�����ļ��������ļ�
	- ǿ�����ܣ����ṩ����֧��
	- ������һ�λ������������������
- innodb���ۼ�����
	- ����ռ䡱�����ļ�����־�ļ�
	-  �ṩ����֧�ֺ�����ȸ߼����ݿ⹦��
	-  ���������������𲽻��������������
		- ���һ��sql������������������MySQL�ͻ�����������������
		- ���һ���������˷�����������MySQL���������÷���������������������������
			 
	    - ����������ͬʱִ�У�һ����ס�������������ڵȴ������������
		- ��һ�������˷������������ڵȴ����������������ͻᷢ��������
		
----------
# MySql�Ż�
 - �����ֶ�Ϊ�������Զ��������
 - not in �滻�� not exists
 - <>Ŀ�� �滻��>Ŀ�� or <Ŀ��
 - �������Ͻ������� year(udate)<2007 �ĳ� udate>"2007-01-01"
 - ɾ������ʹ�û����ʹ�õ�����
 - ������������������ǰ׺
	 - ������(area,age,salary)�ĸ�������
	 - �൱�ڴ�����(area,age,salary)��(area,age)��area��������
 - ʹ��ǰ׺������
	 - ����CHAR(255)���У�ǰ10���ַ��ڣ������ֵ��Ψһ��
 - like���
	 - like "%Ŀ��%" ����ʹ������
	 - like "Ŀ��%" ����ʹ������

 ----------
# KEY   
- PRIMARY KEY��һ��ΨһKEY�����еĹؼ����б��붨��ΪNOT NULL��
- һ����ֻ��һ��PRIMARY KEY
- KEYδ�ض��������KEY������Լ��
    - �Ա����ֶν�������Լ��������ͨ��primary foreign unique�ȴ���
- KEY��Ҫ�������ӿ��ѯ�ٶ�

----------

# ������� 
    CREATE TABLE `PictureTimeInfo` (
      `PictureId` int(11) NOT NULL AUTO_INCREMENT COMMENT '����',
      `TypeId` int(11) NOT NULL COMMENT '��ҵ������',
      `PictureUrl` varchar(500) NOT NULL COMMENT 'ͼƬURL',
      `BeginTime` datetime NOT NULL COMMENT '��ʼʱ��',
      `EndTime` datetime NOT NULL COMMENT '����ʱ��',
      `AddTime` datetime NOT NULL COMMENT '���ʱ��',
      `UpdateTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '����޸�ʱ��',
      PRIMARY KEY (`PictureId`),
      KEY `IX_BeginTime_EndTime` (`BeginTime`,`EndTime`),
      KEY `IX_AddTime` (`AddTime`),
      KEY `IX_UpdateTime` (`UpdateTime`))
    ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8 COMMENT='����ͼ������';


----------
# mysqlִ��˳��

 `[7]`select distinct(s1.spot_id_no),s2.spot_name
`[1]`from point_info s1
`[3]`left join spot_info s2
`[2]`on s1.spot_id_no=s2.spot_id_no
`[4]`where s1.spot_id_no=s2.spot_id_no
`[5]`group by s1.spot_id_no
`[6]`having count(s1.spot_id_no)>10
`[8]`order by s2.spot_name desc
`[9]`limit 0,1

----------
# ���Ӳ�ѯ

 - ������ `join`
 - ������
	 - �������� `left join ` A:�� B:NULL
	 - �������� `right join` A:NULL B:��
	 - ��ȫ���� `full join` �������ֶ���
 - ��������
	
----------
# NoSql

 -  sql�ǻ��ڱ�����ݿ⣬��nosql���ݿ��л����ĵ��ġ���ֵ�Ե�
 -  ���ڸ��ӵĲ�ѯ
 - �ֲ�ε����ݴ洢
 - ACID��CAP�־��ԡ������ԡ�����������

----------
������չ
----------


 - ������չScale-out
	 - ����һ��Сˮ�ף������Աߣ���ͨ
 - ������չ
	 - ��һ����ˮ�ף����㡢ˮ�����²��õ���ˮ��

----------