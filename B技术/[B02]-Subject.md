# 容器装最多

    
	public int getMaxArea(int []nums){
		int len=nums.length;
		int i=0;int j=len-1;
		int ansMax=0;
		while(i<j){
			int curr=(j-i)*Math.min(nums[i],nums[j]);
			ansMax=Math.max(ansMax, curr);
			
			if(nums[i]<nums[j]){
				int left=i;
				while(left<j&&nums[left]<=nums[i]){
					left++;
				}
				i=left;
				continue;
			}
			int right=j;
			while(right>i &&nums[right]>=nums[j]){
				right--;
			}
			j=right;
		}//循环结束
		return ansMax;
	}
    


# 贪心算法

    public int loadOnbackPack(Scanner sc){
		int [][]dp=new int [N][Capa];
		for(int i=1;i<=N;i++){
			for(int volume=1;volume<=Capa;volume++){
				dp[i][volume]=dp[i-1][volume];
				if(weight[i]<=volume){
					dp[i][volume]=Math.max(dp[i][volume], dp[i-1][volume-weight[i]]+money[i]);
				}
			}//内层
		}//外层
		return dp[N][Capa];
	}

	public int loadOnbackPackOne(Scanner sc){
		int []dp=new int [Capa];
		for(int i=1;i<=N;i++){
			for(int volume=Capa;volume>=1;volume--){
				if(weight[i]<=volume){
					dp[volume]=Math.max(dp[volume], dp[volume-weight[i]]+money[i]);
				}
			}//内层
		}//外层
		return dp[Capa];
	}
    

----------
#课程规划
    
    public int[] findOrder(int len,int[][] nums) {
        int[] ansNums = new int[len];
        //坑我的数量
        int[] preCounts = new int[len];
        //我坑了哪些家伙
        List<Set<Integer>> posts = new ArrayList<Set<Integer>>();
        
        for (int i = 0; i < len; i++){
            posts.add(new HashSet<Integer>());
        }
        //[0,1]，0的前修课程是1，1坑了0  
        for (int i = 0; i < nums.length; i++){
            posts.get(nums[i][1]).add(nums[i][0]);
        }
        //0作为value出现的次数
        for (int i = 0; i < len; i++) {
            Set<Integer> set = posts.get(i);
            Iterator<Integer> it = set.iterator();
            while (it.hasNext()) {
                preCounts[it.next()]++;
            }
        }

        for (int i = 0; i < len; i++) {
            int t = 0;
            for (t = 0; t < len; t++) {
                if (preNums[t] == 0) {
                    break;
                }
            }
            if (t == len){
                return new int[0];
            }
            ansNums[i]=t;preCounts[t]=-1;
            Set<Integer> set = posts.get(j);
            Iterator<Integer> it = set.iterator();
            while (it.hasNext()) {
                preNums[it.next()]--;
            }
        }//外层循环结束
        return res;
    }

----------
#回文

   
	public static final int NO=0;
	public static final int YES=1;
	
	public int HuiWengetLen(String str){
		int len=str.length();
		int [][]dp=new int[len][len];
		
		for(int start=0;start<len;start++){
            for(int end=0;end<len;end++){
				if(start>=end){dp[start][end]=YES;}
				else{dp[start][end]=NO;}
			}
		}
		
		int ansLen=1;
		int minLeft=0;int maxRight=0;
		for(int step=1;step<len;step++){
			for(int start=0;start<len-step;start++){
				int end=start+step;
		        if(str.charAt(start)!=str.charAt(end)){
					dp[start][end]=NO;
					continue;
				}
				dp[start][end]=dp[start+1][end-1];
				if(dp[start][end]==YES){
					if(end-start+1>ansLen){
						ansLen=end-start+1;
						minLeft=start;
						maxRight=end;
					}
				}
			}//内层
		}//外层
		return ansLen;
	}
    

----------
#回文分区
 
    	
	public void dfs(int start,String str,int[][]dp,ArrayList<String>tempList,ArrayList<ArrayList<String>>finalAns){
		int len=str.length();
		if(start>len-1){
			finalAns.add(ArrayList<String>(tempList.clone());
			tempList.clear();
			return;
		}
	
		for(int end=start;end<len;end++){
			if(dp[start][end]==YES){
				tempList.add(str.substring(start,end+1));
				int myStart=end+1;
				dfs(myStart,str,dp,tempList,finalAns);
				tempList.remove(tempList.size()-1);
			}
		}//循环end
	}

----------
#LCS

 	public int cacuLongest(int []nums){
		int len=nums.length; 
		int []eNums=new int[len+1];
		int index=1;eNums[index]=nums[0];
		int top=endNums[index];
		
		for(int i=1;i<len;i++){
			int curr=nums[i];
			if(curr>top){index++;eNums[index]=curr;continue;}
			int left=0, right=index;
            left=getBig(curr,eNums,left,right);
            eNums[left] = curr;
		}
		return index;
	}
	
	public int getBig(int target,int []nums,int left,int right){
	    while(left <= right){
            int middle = (left + right) / 2;
            if (nums[middle])<target{ left = middle+1;}
            else{right = middle-1;}
        }
        return left;
	}
    


----------
# 全排列 

  
	public void generateBigger(int []nums){
		int right=len-1;int left=right-1;
		while(left>=0){
		    if(nums[left]<=nums[left+1]){break;}
		    left--;
		}
		if(left<0) { return;}
		
		reverseNums(nums,left,right);
		SwapBig(nums,left,right);
		addList(nums);
		generateBigger(nums);
	}
	
	public void reverseNums(int []nums,int left,int right){
	    int i=left+1;int j=right;
		while(i<=j){swap(nums,i,j);i++;j--;}
	}
	
	public void SwapBig(int []nums,int left,int right){
    	for(int t=left+1;t<=right;t++){
			if(nums[t]>nums[left]){swap(nums,left,t);	break;}
		}
	}


# N对圆括号
   
	public List<String> genrate(int n){
		List<String> ansList=new ArrayList<String>();
		if(n<=0){return ansList;}
		dfs(ansList,"",n,n);
		return ansList;
	}

	public void dfs(List<String>ansList,String curr,int left,int right){
		if(left>right){return;}
		if(left==0&&right==0){ansList.add(curr);}
        if(left>0){dfs(ansList,curr+"(",left-1,right);}
		if(right>0){dfs(ansList,curr+")",left,right-1);}
	}

----------

# BFS-最小替换
- hit->log [hot,dot,dog,lot,log]
- hit->hot->dot->dog->log 长度为5

----------


    
    public int ladderLength(String bWord, String eWord, Set<String> dictSet) {
        HashMap<String, Integer> myMap = new HashMap<String, Integer>();
        myMap.put(bWord, 1);
        Queue<String> queue = new LinkedList<String>();
        queue.offer(bWord);
        
        if (dictSet.contains(bWord)) {dictSet.remove(beginWord);}
        
        while (!queue.isEmpty()) {
            String top = queue.poll();
            int len = top.lenght();
            StringBuilder sb;
            int level = maps.get(top);
            for (int i = 0; i < len; i++) {
                sb = new StringBuilder(top);
                for (char ch = 'a'; ch <= 'z'; ch++) {
                    builder.setCharAt(i, ch);
                    String curr= builder.toString();
                    if (tmpStr.equals(top)) {continue;}
                    if (curr.equals(endWord)) {return level + 1;}
                    if (wordDict.contains(curr)) {
                        queue.offer(curr);
                        maps.put(curr, level + 1);
                        wordDict.remove(curr);
                    }
                }//置换1个ch
            }//置换1个单词
        }//置换队列
        return 0;
    }
    
    

----------
# DFS-能否搜出  
- A B C E 
- S F C S
- A D E E
 - ABCCED和SEE可以 ABCB不可以

----------

  
    int mx;int my;
    Node[] nodes;

    public boolean exist(String myWord,char[][] nums) {
        if (nums.length == 0) {return false;}
        int cx = 0;int cy = 0;
        char[] word = myWord.toCharArray();
        mx = board.length;my = board[0].length;
        nodes = new XY[word.length];
        if (word.length == 0) return false;
        for (cx = 0; cx < mx; cx++) {
            for (cy = 0; cy < my; cy++) {
                if (word[0]==nums[cx][cy]) {
                    nodes[0] = new XY(cx, cy);
                    if (search(word,nums, 1))
                        return true;
                }
            }//y循环
        }//x循环
        return false;
    }

    boolean search(char[] word,char[][] nums,int index) {
        if (index >= word.length){return true;}
        int x = nodes[index - 1].x;int y = nodes[index - 1].y;
        for (Node curr : new Node[]{new Node(x - 1, y), new Node(x + 1, y), new Node(x, y - 1), new Node(x, y + 1)}) {
            int cx = curr.x;int cy =curr.y;
            for (int i = 0; i < index; i++) {
                if (nodes[i].x == cx && nodes[i].y == cy){continue;}
            }
            if (cx >= 0 && cx < mx && cy >= 0 && cy < my) {
                if (word[index]==nums[cx][cy]) {
                    nodes[index] = new XY(cx,cy);
                    if (search(word,nums, index + 1)) {return true;}
                }
            }
        }//循环"前后左右"
        return false;
    }