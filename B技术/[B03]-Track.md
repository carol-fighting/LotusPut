----------


#栈to队列

 - 栈->队列： 栈2：缓存输出
  - 进：只进栈1
  - 出：空，栈1全导入；非空：直接出

----------
- 队列->栈：either one EMPTY
    - 进：只进非空的
    - 出：队列前n-1个元素导入到空的，最后一个元素出

----------
# 论文的引用
- T个论文，t个引用次数>=h;其余T-t个,引用次数小于h
- 初始时h=T,对数组排序，最小的文章>=h,返回h;h--,继续

----------
#栈min()
- 进：栈1正常进；栈2判断是否比当前小
- 出：both正常出

----------
#小偷偷钱
- rob[0]=nums[0];
- rob[1]=Max(nums[0],nums[1]);
- rob[i]=Max(rob[i-1],rob[i-2]+nums[i]);
- 街为环形:Max([0,len-2],[1,len-1])

----------
#找只出现一次
- 5 1 7 5 7
- 4 5 1 7 5 7

----------
	public int getElement(int []nums){
	    int []bitSum=new int[32];
		int len=nums.length;
		for(int i=0;i<len;i++){
			for(int t=0;t<32;t++){
				bitSum[t]+=((nums[i]>>t)&1);
			}
		}
		int ansEle=0;
		for(int t=0;t<32;t++){
			if(bitSum[t]%3!=0){ansEle+=(1<<t);}
		}
		return ansEle;
	}
    

----------
#能否跳到最后
- 每个元素值：表示下一跳的最大长度

----------
   
	public boolean canJumpFinalStep(int []nums)
	{
		int step=nums[0];
		for(int i=1;i<nums.length;i++){
			if(step<=0){ return false;}
		    step--;
			step=Math.max(step, nums[i]);
		}
		return true;
	}
    

----------
# 转移方案数
- 数量为c,罪行值的和<=target

----------
    
	public int getTransferKind(int []nums,int c,int target){
		int ansKind=0;int len=nums.length;
		if(len==0||c>len){return 0;}
		
		for(int i=0;i<len-1-c;i++){
			int mySum=nums[i];
			int step=1;
			for(step=1;step<c;step++){
				int j=i+step;
				if(mySum>target){break;}
				mySum+=nums[j];
			}
			if(step==c){ansKind++;}
		}
		return kindAns;
	}
    

----------


# 和大于target
	public int minSubArrayLen(int []nums,int target){
		int len=nums.length;
		int ansMin=Integer.MAX_VALUE;
		int i=0;int j=0;int mySum=0;
		
		while(i<len && j<len){
			while(mySum<target && j<len){
				mySum+=nums[j];
				j++;
			}
			while(mySum>=target &&i<=j){
				ansMin=Math.min(ansMin,j-i+1);
				mySum-=nums[i];
				i++;
			}
		}
		if(ansMin==Integer.MAX_VALUE){ansMin=0;}
		return ansMin;
	}
    

----------
#各种颜色珠子
   
   
	public int getShortestBeads(int[] nums,int ck){
		int len = beads.length;
		int []flag=new int[ck];
		int i = 0,j = 0;int ansMin = len;
		
		while(j<len){
			while(!canStop()&&j<len){flags[nums[j]]+=1;	j++;}
			while(canStop()){flags[nums[i]]-=1;i++;}
			if(j>len){break;}
		    ansMin=Math.min(ansMin,j-i+1);
		}
		return ansMin;
	}
	
    public boolean canStop(){
		int ans = 1;
		for (int i=0; i<flags.length; i++){ans*=flags[i];}
		return ans!=0?true:false;
	}
    
----------


#最大评分和
- 评分：前方同学包括自己的最小ID,最好降序
-t1不准t2在他前面，若t1小，不管；t2小，更新t2评分

----------


    
	public int  dealAtrip(int len,int [][]nums){
	    int []scores=new int[len+1];
	    for(int i=1;i<len+1;i++){scores[i]=i;}
	    for(int t=0;t<nums.length;t++){
	        int b=nums[t][1];int a=numsp[t][0];
	        scores[b]=Math.min{scores[b],scores[a]};
	    }
	    int ansMin=0;
	    for(int i=1;i<len+1;i++){ansMin+=scores[i];}
	    return ansMin;
	}
    

----------
#通配符递归
- "?":一个 
- "*":0或多个

----------


   
    	public boolean isMatched(String p,String s){
    		if(s==null||p==null){
    			if(s==null&&p==null){return true;}
    			return false;
    		}
    		int i=p.length()-1;int j=j.length()-1;
    		return dfs(p,s,i,j);
    	}
    	
    	public boolean  dfs(String s,String p,int i,int j){
    		if(i<0||j<0){
    			if(i<0&&j<0){return true;}
    			return false;
    		}
    		
    		if(p.charAt(i)=='?'||p.charAt(i)==s.charAt(j)){return dfs(s,p,i-1,j-1);}

    		if(p.charAt(i)=='*'){
    		    int t=j;
    			while(t>=0){
    				if(dfs(s,p,i-1,t--)==true){return true;}
    			}
    			return false;
    		}
    		return false;
    	}
    
#通配符迭代  

    public boolean isMatch(String p,String s){
                //start=i,在s中位置为match
		int i=0;int j=0;
		int match=0;int star=-1;
		while(j<s.length()){
			if(i<p.length()&&(p.charAt(i)==s.charAt(j)||p.charAt(i)=='?')){
				i++;
				j++;
				continue;
			}
			if(i<p.length()&&p.charAt(i)=='*'){
				star=i;
				match=j;
				j++;
				continue;
			}
			if(star!=-1){
				i=star+1;
				match++;
				j=match;
				continue;
			}
			return false;
		}
		while(i<p.length()&&p.charAt(i)=='*'){i++;}
		return i==p.length();
	}
    
----------
#拉票和复习
	public int[] getWork(int [][]time,int total){
		int []nums=new int[len];
		for(int i=0;i<len;i++){
		    nums[i]=time[i][1]-time[t][0];
		    if(nums[i]>total){nums[i]=total;break;}
		    total-=nums[i];
		}
		for(int i=0;i<len;i++){nums[i]+=time[i][0];}
	    return nums;
	}

----------
#二维数组查找
   
	public boolean canFind(int [][]nums,int target){
		int m=nums.length;int n=nums[0].length;
		int tm=0;int tn=n-1;
		while(tn>=0 && tm<=m-1){
			int curr=nums[tm][tn];
			if(curr==target){return true;}
			if(curr<target){tm++;}
			else{tn--;}
		}
		return false;
	}
    
----------
#连续最大和
	public int MaxContinue(int []nums){
		int ans=0;
		int currMax=0;
		for(int i=0;i<nums.length;i++){
			currMax=Math.max(currMax+nums[i], 0);
			ans=Math.max(ans, currMax);
		}
		return ans;
	}

----------
#整数倒置
    public int reverse(int n){
        if(n==Integer.MIN_VALUE){
			return 0;
		}
		if(n>=0){
			int ans=0;
			while(n!=0){
				if(ans>Integer.MAX_VALUE/10){return 0;}
				ans=ans*10+n%10;
				n=n/10;
			}
			return ans;
		}
        return -reverser(-n);
    }


----------
# N皇后
	public void putQueenIndex(int index){
		int r=index;
		for (int c= 0;c<len; c++){
			if (nums[r][c] != YES){continue;}
		    pos[r]=c;
		    lock(c,r);
			if (r==len-1){ansKind=ansKind+1;}
			else{putQueenIndex(r+1);}
			unlock(c,r);
		}
	}
	
	public void lock(int c,int r){
		for (int i = r + 1; i  < len; i ++){ 
			nums[i][c]++;
			if ((c+(i-r))<len){nums[i][c+(i-r)]++;}
			if ((c-(i-r)) >= 0){nums[i][c-(i-r)]++;}
		}
    }
    
    public void unLock(int c,int r){
    	for (int i=r+1; i<len; i++){ 
			nums[i][c]--;
			if ((c+(i-r))<len){nums[i][c+(i-r)]--;}
			if ((c-(i-r)) >= 0){nums[i][c-(i-r)]--;}
		}
    }   

----------


# 青蛙走迷宫


 - 从[0,0]走到[0,c-1]的位置，数组值为0表示阻碍，1表示可走
 - 初始能量为P，水平移动-1，往下走不变，往上走+3

 

----------
	//-1, 0 上 -3
	// 1, 0 下  0
	// 0,-1 左 -1
	// 0, 1 右 -1
	static int[]fx={-1, 1, 0, 0 };  
	static int[]fy={ 0, 0,-1, 1 };  
	static int[]fp={-3, 0,-1,-1 };
	
	public static void getInput(){
		Scanner in = new Scanner(System.in);
		int n = in.nextInt();int m = in.nextInt();int P = in.nextInt();
        int[][] nums = new int[n][m];
        for(int i = 0 ; i < n ; i++){
			for(int j = 0 ; j < m ; j++){
				nums[i][j] = in.nextInt();
			}
		}
        String path=getPath(nums,P);
	}
	
	private static String getPath(int[][] nums, int p) {	
		int r = nums.length;int c = nums[0].length;
		//到达(i,j)时，剩余的最大能量 
		int[][] rNums=new int[r][c]; 
		//到达(i,j)时，path
		String [][]sNums=new String[r][c];
		
		dfs(0,0,p,getAns(0,0),nums,rNums,sNums);
		//到达(0,c-1)时，能量是否满足，返回最后的path
        if(rNums[0][c-1] >= 0){return sNums[0][c-1];}
        return "Can not escape!";
	}
	
	public static String getAns(int i,int j){return ",["+i+","+j+"]";}
	
	private static void dfs(int i, int j, int p,String ans,int[][] nums,int[][] rNums,String[][] sNums){
		if(i<0||i>= nums.length||j<0||j>= nums[0].length){return;}
		if(nums[i][j]!=1||p<0){return;}
		//如果p<剩余的最大能量，return
		if(p < rNums[i][j]){return;}
		//更新剩余的最大能力
		rNums[i][j] = p;
		//更新path
		sNums[i][j] = ans;
		
		for(int t=0;t<4;t++){
			String currAns=ans+getAns(i+fx[t],j+fy[t]);
			dfs(i+fx[t],j+fy[t],p+fp[t],ans+getAns(i+fx[t],j+fy[t]),nums,rNums,sNums);
		}
	}
	
----------
#403禁止访问

 - N条规则：allow/deny ip地址 或ip地址/子网掩码
 - M条请求：ip地址
 - 若没有匹配到any规则，允许访问

----------


    
   
    	
	public String toBinaryIps(String ipStr){
		String bIp="";
		String [] part=ipStr.split("\\.");
		for(int i=0;i<part.length;i++){
			int num=Integer.valueOf(part[i]);
			bIp +=toBinaryIp(num);
		}
		return bIp;
	}
	
	public String toBinaryIp(int num){
		String str=Integer.toBinaryString(num);
		while(str.length()<8){str="0"+str;}
		return str;
	}
	
	public void doSave(String rule){
		String flag=rule.split(" ")[0];
		String ipAll=currStr.split(" ")[1];
		String []ipNums=ipAll.split("/");
		String bIp=toBinaryIp(ipNums[0]);
		int mask=32;
		if(ipNums.length>1){mask=Integer.valueOf(ipNums[1]);}
		bIp=bIp.subString(0,mask);
		doPush(bIp,mask,flag);
	}
	
	public void doPush(String bIp,int mask,String flag){
		if(rMap.containsKey("zero")){return;}
		for(int t=0;t<mask;t++){
			if(rMap.containsKey(bIp.substring(0,t))){return;}
		}
	    
	    boolean state=true;
	    if(flag.equals("deny")){state=false;}
	
		if(mask==0){rMap.put("zero", state);}
		else{rMap.put(bIp,state);}
	}

	public boolean doMatch(String ip){
		String bIp=toBinaryIps(ip);
		for(int mask=32;mask>=0;mask--){
			String key=bIp.substring(0, mask);
			if(rMap.containsKey(key)){return rMap.get(key);}
		}
		return true;
	}


----------
#股票买卖
 - 无限次买卖
    - 从第二天起：如果今天>昨天，ans+=(今天-昨天)

 - 一次买卖
    - 默认从第一天买，获利0,从第二天起，更新bIndex或ans 

 - 两次买卖
 

----------


    public int dealTwo(int []nums){
		int bIndex=0;int sIndex=len-1;
		int []pLeft=new int[len];int []pRight=new int[len];
		int ans=0;
		
		for(int i=1;i<len;i++){
			if(nums[i]<nums[bIndex]){
				bIndex=i;
				contiune;
			}
			ans=Math.max(ans,nums[i]-nums[bIndex]);     
			p1[i]=ans;
		}
	
		ans=0;
		for(int j=len-2;j>-1;j--){
			if(nums[j]>nums[sellDay]){
				sIndex=j;
				continue;
			}
			ans=Math.max(ans,nums[sellDay-nums[j]]);
			p2[j]=ans;
		}
		
		ans=0;
		for(int i=0;i<len;i++){ans=Math.max(ans,p1[i]+p2[i]);}
		return ans;
	}

----------


 - 冷冻期
 

----------


    public int getStockWithfreeze(int []nums){
		int len=nums.length;
		int []buy=new int[len];int []sell=new int[len];
		
		buy[0]=0-nums[0];buy[1]=Math.max(buy[0], 0-nums[1]);
		sell[0]=0;sell[1]=Math.max(sell[0],nums[1]-nums[0]);
		
		for(int i=2;i<len;i++){
		    buy[i]=Math.max(buy[i-1], sell[i-2]-nums[i]);	
			sell[i]=Math.max(sell[i-1], buy[i-1]+nums[i]);
		}
	    return sell[len-1];
	}