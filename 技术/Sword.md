#约瑟夫环

 - 一共有n个人，从0到n-1报数，报到m-1的人退出，求谁留下
 - f(i)表示i个人玩时，留下的人的编号

----------

 - 第一轮：(m-1)%n退出，第二轮开始编号k=m%n
 - 映射后为：0到n-2，映射前为：k到k-2
 - 若第二轮，胜出者为x，在第一轮是(x+k)%n，是(x+m)%n
 - f(i)=(f(i-1)+m)%n f(1)=0
 
----------
# 和为target
- 两个指针：i数组的开头,j指向数组的结尾
- 两个指针：i指向序列的首元素，j指向序列的尾元素，循环
- 排序数组，一直到small=(1+target)/2

----------
#最小的K个数

 - 基于快排的思路，O(N)时间复杂度
 - 基于数组的第k个数字进行调整，比第k个数字小的（左边）
 
----------
    public void getLeastK(int []nums,int n,int k){
		if(nums==null||n<=0||k<=0||k>n){return;}
		int start=0;int end=n-1;
		int index=getIndex(nums,start,end);
		while(index!=k-1){
			if(index>k-1){
				end=index-1;
				index=getIndex(nums,start,end);
			}
			else{
				start=index+1;
				index=getIndex(nums,start,end);
			}
		}//循环
	}


----------
# 数组的乘积

    //1、定义
    left[i]=nums[0]*nums[1]*nums[i-1];
    right[i]=nums[i+1]*nums[i+2]*nums[n-1];
    
    //2、计算
    left[i]=left[i-1]*nums[i-1];
    right[i]=right[i+1]*nums[i+1];
      

----------

#找重复[0,len-1]
    public boolean isDup(int []nums){
        for(int i=0;i<len;i++){  
            while(nums[i]!=i){  
                if(nums[i]==nums[nums[i]]){return true;}
                swap(nums,i,nums[i]); 
            }  
        }  
        return false;  

----------


#滑动窗口最大值

	public List maxInWindows(int [] num, int size){
		ArrayList ansList=new ArrayList<>();
		LinkedList<Integer> indexDeque = new LinkedList<Integer>();
		if (num == null){return ansList;}
		if (num.length < size || size < 1){return ansList;}
		
		for (int i = 0; i < size - 1; i++) {
			while (!indexDeque.isEmpty() && num[i] >num[indexDeque.getLast()]) {indexDeque.removeLast();}
			indexDeque.addLast(i);
	    }
	
		for (int i = size - 1; i < num.length; i++) {
	       while (!indexDeque.isEmpty() && num[i] >num[indexDeque.getLast()]) {indexDeque.removeLast();}
	       indexDeque.addLast(i);
	
	       if (i-indexDeque.getFirst()+1>size) {indexDeque.removeFirst();}
	       ansList.add(num[indexDeque.getFirst()]);
	    }
		return ansList;

	}


#中序遍历next



    public TreeNode getNext(TreeNode node){
		if(node==null){return null;}
		TreeNode ans=null;
		
		if(node.right!=null){
			TreeNode temp=node.right;
			while(temp.left!=null){temp=temp.left;}
			ans=temp;
			return ans;
		}
		if(node.parent!=null){
			TreeNode temp=node;
			TreeNode parent=temp.parent;
			while(parent!=null&&temp==parent.right){
				temp=parent;
				parent=parent.parent;
			}
			ans=parent;
		}
		return ans;
	}
	
----------
#链表去重
    public void deleteDepulication(ListNode head){
		if(head==null){return;}
		ListNode curr=head;ListNode pre=null;
		boolean delete=false;
		while(curr!=null){
			ListNode next=curr.next;
			delete=false;
			if(next!=null&&curr.val==next.val){delete=true;}
			if(!delete){
				pre=curr;
				curr=curr.next;
				continue;
			}
			//中间的这一串，我都不要了
			ListNode temp=curr;
			while(temp!=null&&temp.val==curr.val){
				temp=temp.next;
				next=temp;
			}
			//跟前面接上
			if(pre==null){head=next;}
			else{pre.next=next;}
			curr=next;
		}//循环
	}
	

----------
#能否顺子
    //把大王、小王看成0
    public boolean isContinuous(int []nums){
		int len=5;
		int []temp=new int[len];
		int zeroCount=0;
		int minValue=Integer.MAX_VALUE;
		
		//第一次遍历：找0,找非0最小
		for(int i=0;i<len;i++){
			if(nums[i]==0){zeroCount++;continue;}
			minValue=Math.min(minValue, nums[i]);
		}
		
		//第二次遍历:统计temp数组和ans
		int ans=zeroCount;
		for(int i=0;i<len;i++){
			int index=nums[i]-minValue;
			if(index<0||index>=len-1){return false;}
			if(temp[index]!=0){return false;}
			temp[index]++;
			ans++;
		}
		
		if(ans==len){return true;}
		return false;
	}

----------
#n个骰子点数和

	int minV=6;
	
	public void printPro(int n){
		int []nums=new int[minV*-minV+1];
		getPro(n,nums);
	}
	
	public void getPro(int n,int []nums){
		for(int v=1;v<=minV;v++){
			doPro(n,n,v,nums);
		}
	}
	
	public void doPro(int n,int index,int sum,int []nums){
		if(index==1){nums[sum-n]++;return;}
		for(int v=1;v<minV;v++){
			doPro(n,index-1,sum+v,nums);
		}
	}

    //way-2
	public void printPro(int n){
		int [][]nums=new int[2][maxV*+1];
		int state=0;
		for(int v=0;v<maxV*n;v++){
			nums[state][v]=1;
		}
		//考察第k个骰子
		for(int k=2;k<=n;k++){
			//不合理，因为至少是k
			for(int s=1;s<k;s++){nums[1-state][s]=0;}
			//对于每一个和
			for(int s=k;s<maxV*n;s++){
				for(int j=1;j<=s;j++){
					nums[1-state][s]=nums[state][s-j];
				}//加上state下每一个可能的和
			}//循环每一个s
			state=1-state;
		}
		//最后state保存的结果即为所求
	}


----------

#二叉树深度

 - 简单的做法：一个节点会被重复遍历多次
 - 利用后序遍历，每次返回一下他的深度吧

----------
	public boolean doJudge(TreeNode root,int []nums){
		if(root==null){nums[0]=0;return true;}
		int []lNums=new int[1];int []rNums=new int[1];
		if(doJudge(root.left,lNums)&&doJudge(root.right,rNums)){
			int diff=lNums[0]-rNums[0];
			if(diff<=1&&diff>=-1){
	        	nums[0]=(lNums[0]>rNums[0]?lNums[0]:rNums[0])+1;
				return true;
			}//自己平衡
		}
		return false;
	}
	
----------
#排序数组次数

    public int getFirstIndex(int target,int []nums,int start,int end){
		if(start>end){return -1;}
		int m=(start+end)/2;
		if(nums[m]==target){
			if(m==0){return m;}
			if(m>0 && nums[m-1]!=target){return m;}
			end=m-1;
		}
		else{
			if(nums[m]>target){end=m-1;}
			else{start=m+1;}
		}
		return getFirstIndex(target,nums,start,end);
	}
	
	public int getLastIndex(int target,int[]nums,int start,int end){
		if(start>end){return -1;}
		int m=(start+end)/2;
		if(nums[m]==target){
			if(m==nums.length-1){return m;}
			if(m>0 && nums[m-1]!=target){return m;}
			end=m+1;
		}
		else{
			if(nums[m]>target){end=m-1;}
			else{
				start=m+1;
			}
		}
		return getLastIndex(target,nums,start,end);
	}
	
----------
#数组的逆序对

	public int doFind(int []nums,int[]temp,int start,int end){
		if(start==end){temp[start]=nums[start];return 0;}
		int mLen=(end-start)/2;
		
		//注意：将temp赋值给nums
		int p1=doFind(temp,nums,start,start+mLen);
		int p2=doFind(temp,nums,start+mLen+1,end);
		
		int i=start+mLen;int j=end;int k=end;int ans=0;
		
		while(i>=start&&j>=start+mLen+1){
			if(nums[i]>nums[j]){
				i--;
				temp[k--]=nums[i];
				ans+=j-(start+mLen+1)+1;
			}
			else{
				j--;
				temp[k--]=nums[j];
			}
		}//循环
		
		for(int h=i;h>=start;h--){temp[k--]=nums[h];}
		for(int h=j;h>=start+mLen+1;h--){temp[k--]=nums[h];}
		return p1+p2+ans;
	}
	
----------
#1-n中1的次数

	public int doCount(char []nums,int start){
		if(nums==null||nums.length==0||start==nums.length){return 0;}
		int len=nums.length;
		int fr=nums[start]-'0';
		if(len==1){
		    if(fr==0){return 0;}}
			return 1;
		}
		//出现在最高位
		int ans=0;
		if(fr>1){ans=(int)Math.pow(10, len-1);}
		else{ans=1+atoi(nums,start+1);}
		
		//除最高位的其他位中1的总数
		int ans=fr*(int)Math.pow(10, len-2);
	
		return ans+doCount(nums,start+1);
	}

----------



#字符串排列

    public void getPerm(char []nums){
		if(nums==null||nums.length==0){return;}
		doPerm(nums,0);
	}
	
	public void doPerm(char []nums,int start){
		if(start==nums.length){
			System.out.println(nums);
			return ;
		}
		for(int i=start;i<nums.length-1;i++){
			swap(nums,start,i);
			doPerm(nums,start+1);
			swap(nums,start,i);
		}
	}
	
----------
#复杂链表复制
    public void step1(ComplexNode root){
        ComplexNode w=root;
        while(w!=null){
            //被克隆出来的：temp
            ComplexNode temp=new ComplexNode(w.val);
            temp.next=w.next;
            temp.slib=null;
            w.next=temp;
            w=w.next.next;
        }
    }
    
    public void step2(ComplexNode root){
        ComplexNode w=root;
        while(w!=null){
            ComplexNode temp=wNode.next;
            if(w.slib!=null){w.slib=temp.slib.next;}
            w=w.next.next;
        }
    }
    
    public void step3(ComplexNode root){
        ComplexNode w=root;
        ComplexNode head=null;
        ComplexNode temp=null;
        
        if(w!=null){
            head=temp=w.next;
            w=w.next.next;
        }
        while(w!=null){
            temp.next=w.next;
            temp=temp.next;
        }
    }


----------
#和为target的路径
   
    public void findPath(TreeNode root,int curr,int target,Stack<Integer>stack){
        curr+=root.val;
        stack.push(root.val);
        
        boolean isLeaf=(root.left==null&&root.right==null);
        if(curr==target&&isLeaf){ doPrint(path);}
        
        if(root.left!=null){findPath(root.left,curr,target,stack);}
        if(root.right!=null){findPath(root.right,curr,target,stack);}
        
        stack.pop(); 
    }

----------
#二叉搜索树的后序

    public boolean isSquence (int []nums,int start,int step){
        if(nums==null||nums.length==0){return false;}
        int root=nums[len-1];int i=start;
        
        for(i=start;i<(len-1);i++){if(nums[i]>root){break;}}
        for(int j=i;j<len-1;j++){if(nums[j]<root){return false;}}
        
        boolean left=ture;
        if(i>0){left=doVerify(nums,0,i);}
            
        boolean right=true;
        if(i<len-1){right=doVerify(nums,i,len-1-i);}
        
        return left&&right;
    }
    
----------
#是否栈的弹出
    public boolean isPopOrder(int []nums1,int []nums2,int len){
        if(nums1==null||nums2==null||len==0){return ans;}
        Stack<Integer> stack=new Stack<Integer>();
        int i=0;int j=0;
        while(i<len){
            int target=nums2[j];
            while(stack.isEmpth()||stack.top()!=target){
                if(i==len){return false;}//全入栈
                stack.push(nums[i]);
                i++;
            }//内层
            stack.pop();
            j++;
        }//外层
        if(stack.isEmpty()&&i==len&&j==len){return true;}
        return false;
    }
    

----------
#二叉树镜像
    public void doMirror(TreeNode root){
        if(root==null){return ;}
        if(root.left==null && root.right==null){return;}
        
        TreeNode temp=root.left;root.left=root.right;root.right=temp;
        
        if(root.left!=null){doMirror(root.left)};
        if(root.right!=null){doMirror(root.right)};
    }
    
----------
#树的子结构
     public boolean isSubTree(TreeNode rootA,TreeNode rootB){
        boolean ans=false;int target=rootB.val;
        if(rootA!=null &&rootB!=null){
            if(rootA.val==target){ans=doIsSame(rootA,rootB);}
            if(!ans){ans=isSubTree(rootA.left,rootB);}
            if(!ans){ans=isSubTree(rootA.right,rootB);}
        }
        return ans;
    }
    
    public boolean doIsSame(TreeNode rootA,TreeNode rootB){
        if(rootA==null||rootB==null){return false;}
        if(rootA.val!=rootB.val){return false;}
        return doIsSame(rootA.left,rootB.left)&&doIsSame(rootA.right,rootB.right);
    }
    
----------
#合并排序链表

    public ListNode Merge(ListNode p1,ListNode p2){
        if(p1==null){return p2;}
        if(p2==null){return p1;}
        ListNode head=null;
        
        if(p1.val<p2.val){
            head=p1;
            head.next=Merge(p1.next,p2);
            return head;
        }
        if(p1.val>p2.val){
            head=p1;
            head.next=Merge(p1.next,p2);
        }
        else{
            head=p2;
            head.next=Merge(p1,p2.next);
        }
        return head;
    }



----------
#链表倒数第k个
- k=0或k>len
- 先走k-1步，当他走到n时，p2=n-(j-1)

----------

    public ListNode findK(ListNode head,int k){
        if(head==null||k==0){return null;}
        ListNode p1=head;
        for(int i=0;i<(k-1);i++){
            if(p1.next==null){return null;}
            p1=p1.next;
        }
        ListNode p2=head;
        while(p1.next=null){
            p1=p1.next;
            p2=p2.next;
        }
        return p2;
    }
    
----------
#调整数组可扩展性
    interface MyObject{
        public isLeftPart(int []nums,int index);
    }
    
    public void Reorder(int []nums,MyObject obj){
        if(nums==null||nums.length==0){return;}
        int i=0;int j=len-1;int pivot=nums[i];
        while(i<j){
            while(i<j&&!obj.isLeftPart(nums,j)){j--;}
            nums[i]=nums[j];
            while(i<j&&obj.isLeftPart(nums,i)){i++;}
            nums[j]=nums[i];
        }
        nums[i]=pivot;
    }

----------
#1到最大的n位数
    public void doMax(int []nums,int len,int index){
        if(index==len-1){print(nums);return;}
        for(int i=0;i<10;i++){
            nums[index+1]='0'+i;
            doMax(nums,n,index+1);
        }
    }
    
    public void print(int n){
        if(n<=0){return;}
        char []nums=new char [n];
        for(int i=0;i<10;i++){nums[0]='0'+i;doMax(nums,n,0);}
    }
    
----------
#数值的整数次方
    boolean state=true;
    public double Power(dobule b,int e){
        if(e<0&&isEqual(0.0,b)){
            state=false;
            return -1;
        }
        int absE=e>0?e:-e;
        int ans=getPower(b,e);
        if(e<0){
            ans=1/ans;
        }
        return ans;
    }
    
    double getPower(double b,int e){
        double ans=1.0;
        for(int i=1;i<=e;i++){
            ans=ans*b;
        }
        return ans;
    }
 

----------
#二进制中1的个数

    public int getNumberOne(int n){
        int ans=0;
        while(n!=0){
            if(n&1!=0){ans++;}
            n=n>>1;
        }
        return ans;
    }
    
    //way-2
    public int getNumberOne(int n){
        int ans=0;
        while(n!=0){
            ++ans;
            n=(n-1)&n;
        }
        return ans;
    }
    
----------
#斐波那契
    public int getFibonacci(int n){
        int nums[2]={0,1};
        if(n<2){return nums[n];}
        int fN=0;int fOne=1;int fTwo=0;
        for(int i=2;i<n+1;i++){
            fN=fOne+fTwo;
            fOne=fN;
            fTwo=fOne;
        }
        return fN;
    }

----------
#旋转数组的最小

    public int findMin(int []nums){
		int len=nums.length();
		int p1=0;int p2=len-1;int m=0;
		while(nums[p1]>=nums[p2]){
		    int m=(p1+p2)/2;
		    if(p2==p1+1){m=p2;break;}
		    if(nums[p1]==nums[m]&&nums[m]==nums[p2]){return doSeq(nums,p1,p2);}//顺序查找
		    if(nums[m]>=nums[p1]){p1=m;continue;}
		    if(nums[m]<=nums[p2]){p2=m;}
		}
		return nums[m];
	}