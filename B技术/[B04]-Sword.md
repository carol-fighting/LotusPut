#Լɪ��

 - һ����n���ˣ���0��n-1����������m-1�����˳�����˭����
 - f(i)��ʾi������ʱ�����µ��˵ı��

----------

 - ��һ�֣�(m-1)%n�˳����ڶ��ֿ�ʼ���k=m%n
 - ӳ���Ϊ��0��n-2��ӳ��ǰΪ��k��k-2
 - ���ڶ��֣�ʤ����Ϊx���ڵ�һ����(x+k)%n����(x+m)%n
 - f(i)=(f(i-1)+m)%n f(1)=0
 
----------
# ��Ϊtarget
- ����ָ�룺i����Ŀ�ͷ,jָ������Ľ�β
- ����ָ�룺iָ�����е���Ԫ�أ�jָ�����е�βԪ�أ�ѭ��
- �������飬һֱ��small=(1+target)/2

----------
#��С��K����

 - ���ڿ��ŵ�˼·��O(N)ʱ�临�Ӷ�
 - ��������ĵ�k�����ֽ��е������ȵ�k������С�ģ���ߣ�
 
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
		}//ѭ��
	}


----------
# ����ĳ˻�

    //1������
    left[i]=nums[0]*nums[1]*nums[i-1];
    right[i]=nums[i+1]*nums[i+2]*nums[n-1];
    
    //2������
    left[i]=left[i-1]*nums[i-1];
    right[i]=right[i+1]*nums[i+1];
      

----------

#���ظ�[0,len-1]
    public boolean isDup(int []nums){
        for(int i=0;i<len;i++){  
            while(nums[i]!=i){  
                if(nums[i]==nums[nums[i]]){return true;}
                swap(nums,i,nums[i]); 
            }  
        }  
        return false;  

----------


#�����������ֵ

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


#�������next



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
#����ȥ��
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
			//�м����һ�����Ҷ���Ҫ��
			ListNode temp=curr;
			while(temp!=null&&temp.val==curr.val){
				temp=temp.next;
				next=temp;
			}
			//��ǰ�����
			if(pre==null){head=next;}
			else{pre.next=next;}
			curr=next;
		}//ѭ��
	}
	

----------
#�ܷ�˳��
    //�Ѵ�����С������0
    public boolean isContinuous(int []nums){
		int len=5;
		int []temp=new int[len];
		int zeroCount=0;
		int minValue=Integer.MAX_VALUE;
		
		//��һ�α�������0,�ҷ�0��С
		for(int i=0;i<len;i++){
			if(nums[i]==0){zeroCount++;continue;}
			minValue=Math.min(minValue, nums[i]);
		}
		
		//�ڶ��α���:ͳ��temp�����ans
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
#n�����ӵ�����

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
		//�����k������
		for(int k=2;k<=n;k++){
			//��������Ϊ������k
			for(int s=1;s<k;s++){nums[1-state][s]=0;}
			//����ÿһ����
			for(int s=k;s<maxV*n;s++){
				for(int j=1;j<=s;j++){
					nums[1-state][s]=nums[state][s-j];
				}//����state��ÿһ�����ܵĺ�
			}//ѭ��ÿһ��s
			state=1-state;
		}
		//���state����Ľ����Ϊ����
	}


----------

#���������

 - �򵥵�������һ���ڵ�ᱻ�ظ��������
 - ���ú��������ÿ�η���һ��������Ȱ�

----------
	public boolean doJudge(TreeNode root,int []nums){
		if(root==null){nums[0]=0;return true;}
		int []lNums=new int[1];int []rNums=new int[1];
		if(doJudge(root.left,lNums)&&doJudge(root.right,rNums)){
			int diff=lNums[0]-rNums[0];
			if(diff<=1&&diff>=-1){
	        	nums[0]=(lNums[0]>rNums[0]?lNums[0]:rNums[0])+1;
				return true;
			}//�Լ�ƽ��
		}
		return false;
	}
	
----------
#�����������

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
#����������

	public int doFind(int []nums,int[]temp,int start,int end){
		if(start==end){temp[start]=nums[start];return 0;}
		int mLen=(end-start)/2;
		
		//ע�⣺��temp��ֵ��nums
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
		}//ѭ��
		
		for(int h=i;h>=start;h--){temp[k--]=nums[h];}
		for(int h=j;h>=start+mLen+1;h--){temp[k--]=nums[h];}
		return p1+p2+ans;
	}
	
----------
#1-n��1�Ĵ���

	public int doCount(char []nums,int start){
		if(nums==null||nums.length==0||start==nums.length){return 0;}
		int len=nums.length;
		int fr=nums[start]-'0';
		if(len==1){
		    if(fr==0){return 0;}}
			return 1;
		}
		//���������λ
		int ans=0;
		if(fr>1){ans=(int)Math.pow(10, len-1);}
		else{ans=1+atoi(nums,start+1);}
		
		//�����λ������λ��1������
		int ans=fr*(int)Math.pow(10, len-2);
	
		return ans+doCount(nums,start+1);
	}

----------



#�ַ�������

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
#����������
    public void step1(ComplexNode root){
        ComplexNode w=root;
        while(w!=null){
            //����¡�����ģ�temp
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
#��Ϊtarget��·��
   
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
#�����������ĺ���

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
#�Ƿ�ջ�ĵ���
    public boolean isPopOrder(int []nums1,int []nums2,int len){
        if(nums1==null||nums2==null||len==0){return ans;}
        Stack<Integer> stack=new Stack<Integer>();
        int i=0;int j=0;
        while(i<len){
            int target=nums2[j];
            while(stack.isEmpth()||stack.top()!=target){
                if(i==len){return false;}//ȫ��ջ
                stack.push(nums[i]);
                i++;
            }//�ڲ�
            stack.pop();
            j++;
        }//���
        if(stack.isEmpty()&&i==len&&j==len){return true;}
        return false;
    }
    

----------
#����������
    public void doMirror(TreeNode root){
        if(root==null){return ;}
        if(root.left==null && root.right==null){return;}
        
        TreeNode temp=root.left;root.left=root.right;root.right=temp;
        
        if(root.left!=null){doMirror(root.left)};
        if(root.right!=null){doMirror(root.right)};
    }
    
----------
#�����ӽṹ
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
#�ϲ���������

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
#��������k��
- k=0��k>len
- ����k-1���������ߵ�nʱ��p2=n-(j-1)

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
#�����������չ��
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
#1������nλ��
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
#��ֵ�������η�
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
#��������1�ĸ���

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
#쳲�����
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
#��ת�������С

    public int findMin(int []nums){
		int len=nums.length();
		int p1=0;int p2=len-1;int m=0;
		while(nums[p1]>=nums[p2]){
		    int m=(p1+p2)/2;
		    if(p2==p1+1){m=p2;break;}
		    if(nums[p1]==nums[m]&&nums[m]==nums[p2]){return doSeq(nums,p1,p2);}//˳�����
		    if(nums[m]>=nums[p1]){p1=m;continue;}
		    if(nums[m]<=nums[p2]){p2=m;}
		}
		return nums[m];
	}