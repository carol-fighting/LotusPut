#排序树恢复
---
    public void doAdjust(int []nums){
        int first=-1;int second=-1;
        for(int i=1;i<len;i++){
            if(nums[i-1]<nums[i]){continue;}
            if(first==-1){first=i-1;}
            second=i;
        }
        swap(nums,first,second);
    }
    
    public void dfs(TreeNode root,TreeNode lastNode){
        if(root.left!=null){dfs(root.left,lastNode);}
        if(lastNode!=null){
            if(lastNode.val>root.val){
                if(first==null){fist=lastNode;}
                second=root;
            }
        }
        lastNode=root;
        if(root.right!=null){dfs(root.right,lastNode);}
    }
    
----------
#搜索转双中序
---

    public static TreeNode ActNode(TreeNode root, TreeNode lastNode){
    	if (root == null){return root;}
    	if (root.left != null){lastNode=ActNode(root.left, lastNode);}
    	root.left = lastNode;
    	if (lastNode != null){lastNode.right = root;}
    	lastNode = root;
    	if (root.right != null){lastNode=ActNode(root.right, lastNode);}
    	return lastNode;
    }
    
----------
#前-中确定树
---
	public TreeNode constructTree(int []preNums,int []inNums,int len){
		if(preNums==null||inNums==null||len==0){return null;}
		return doConstruct(preNums,0,len-1,inNUms,0,len-1);
	}
	
	public TreeNode doConstruct(int []preNums,int s1,int e1,int []inNums,int s2,int e2){
		int rv=preNums[s1];
		TreeNode root=new TreeNode(rv);
		if(s1==e1&&s2==e2){return root;}
		int m=dofind(rv,inNums,s2,e2);
		root.left=doLeft(m,s1,e1,s2,e2,preNums,inNums);
		root.right=doRight(m,s1,e1,s2,e2,preNums,inNums);
		return root;
	}
	
	public int dofind(int rv,int []inNums,int s2,int e2){
		for(int i=s2;i<=e2;i++){if(inNums[i]==rv){return i;}}
		return -1;
	}
	
	public TreeNode doLeft(int m,int s1,int e1,int s2,int e2,int []preNums,int []inNums){
		//中序：[左]根右 
		s2=s2;e2=m-1;int len=e2-s2+1;
		//前序：根[左]右
		s1=s1+1;e1=s1+(len-1);
		return doConstruct(preNums,s1,e1,inNums,e1,e2);
	}
	 
	public TreeNode doRight(int m,int s1,int e1,int s2,int e2,int []preNums,int []inNums){
		//中序：左根[右]  
		s2=m+1;e2=e2;
		int len=e2-s2+1;
		//前序：根左[右]
		e1=e1;s1=e1-(len-1);
		return doConstruct(preNums,s1,e1,inNums,e1,e2);
	}


# 头插法
---
	public ListNode reverse(ListNode root){
		ListNode head=new ListNode(0);
		head.next=null;
		
		ListNode w=root;
		while(w!=null){
			ListNode temp=new ListNode(w.val);
			temp.next=head.next;
			head.next=temp;
			
			w=w.next;
		}
		return head.next;
	}



      
----------
#正反序列化
---
    public TreeNode deserialize(String data) {
        if (data==null||data.length()==0){
            return null;
        }
        String[] vals = data.split(",");
        int len=vals.length;
        int[] nums = new int[len];
        TreeNode[] ansNodes = new TreeNode[len];
        
        for (int i = 0; i < len; i++) {
            if (i > 0) {
                nums[i] = nums[i - 1];
            }
            if (vals[i].equals("null")) {
                ansNodes[i] = null;
                nums[i]++;
            } else {
                ansNodes[i] = new TreeNode(Integer.parseInt(vals[i]));
            }
        }
        for (int i = 0; i <len; i++) {
            if (ansNodes[i] == null) {
                continue;
            }
            ansNodes[i].left = ansNodes[2 * (i - nums[i]) + 1];
            ansNodes[i].right = ansNodes[2 * (i - nums[i]) + 2];
        }
        return ansNodes[0];
    }
    
----------
# 层次遍历
---
    public static ArrayList<ArrayList<Integer>> everyLevel(TreeNode root){
    	ArrayList<ArrayList<Integer>> finalAns = new ArrayList<ArrayList<Integer>>();
    	if (root == null){
    	    return finalAns;
        }
       	Queue<TreeNode> queue = new LinkedList<TreeNode>();
    	ArrayList<Integer> tempList = new ArrayList<Integer>();
    	queue.add(root);
        int nowLevCnt = 1;
    	int nextLevCnt = 0;	
    	
        while (!queue.isEmpty()){
    		TreeNode curr = queue.remove();
    		nowLevCnt--;
    		tempList.add(curr.val);
    		if (curr.left != null){
    			queue.add(curr.left);
    			nextLevCnt++;
    		}
    		if (curr.right != null){
    			queue.add(curr.right);
    			nextLevCnt++;
    		}
    		if (nowLevCnt == 0){
    			nowLevCnt=nextLevCnt;
    			nextLevCnt = 0;
    			finalAns.add((ArrayList<Integer>) tempList.clone());
    			tempList.clear();
    		}
    	}
    	return finalAns;
    }

----------
# 前中后序
---
	public static void preOrder(TreeNode root){
		if(root==null){
		    return ;
	    }
		Stack<TreeNode> stack=new Stack<TreeNode>();
		while(root!=null||!stack.isEmpty()){
			while(root!=null){
				//前序：根
				stack.push(root);
				root=root.left;
			}
			if(!stack.isEmpty()){
				root=stack.pop();
				//中序：根
				root=root.right;
			}
			if(root.right!=null&&map.get(root)!=2){
    			stack.push(root);
    			map.put(root,2);
    			root=root.right;
    		}
    		else{
    			//后序：根
    			root=null;
    		}
		}//while结束
	}

----------

# 是否存在环
---

    public Node hasCircle(Node head){
        Node fast=head;
        Node slow=head;
        Node encounter=null;
        while(fast!=null && fast.next!=null){
            fast=fast.next.next;
            slow=slow.next;
            if(fast==slow){
                encounter=fast;
                return true;
            }
        }
        return false;
    }
    //p1退到起点
    public Node findEntry(Node head,Node encounter){
        Node p1=head;
        Node p2=encounter;
        while(p1!=p2){
            p1=p1.next;
            p2=p2.next;
        }
    }

----------
# 二分搜索
---

    public int binarySearch(int []nums,int target,int left,int right){
    	if(left>right){
    		return -1;
    	}
    	int middle=(left+right)/2;
    	if(target==nums[middle]){
    		return middle;
    	}
    	else if(target>nums[middle]){
    	    return binarySearch(nums,target,middle+1,right);
    	}
    	else{
    	    return binarySearch(nums,target,left,middle-1);
    	}
    }


----------
# 归并排序
---
    public void mSort(int[] nums, int left, int right){
    	if (left < right){
    		int middle = (left + right) / 2;
            mSort(nums, left, middle);
    	    mSort(nums, middle + 1, right);
            doMerge(nums, left, middle, right);
    	}
    }
    
    public void doMerge(int[] nums, int left, int middle, int right){
    	int[] temp = new int[right - left + 1];
    	int i = left,int j = middle + 1;
    	int m = middle,int n = right;
        int k = 0;
    
    	while (i <= m && j <= n){
    		if (nums[i] < nums[j]){
    			temp[k++] = nums[i++];
    		}
    		else{
    			temp[k++] = nums[j++];
    		}
    	}
    	while (i <= m){
    		temp[k++] = nums[i++];
    	}
        while (j <= n){
    		temp[k++] = nums[j++];
    	}
    	for (i = left; i <= right; i++){
    		nums[i] = temp[i - left];
    	}
    }

----------

# 快速排序
---

    public void qSort(int[] nums, int left, int right){
    	if (left < right){
    		int index = getIndex(nums, left, right);
    		qSort(nums, left, index - 1);
    		qSort(nums, index + 1, right);
    		//9.2.11.2
    	}
    }
    public int getIndex(int[] nums, int left, int right){
    	int pivot = nums[left];
    	while (left < right){
    		while (left < right && nums[right] >= pivot){
    			right--;
    		}
    		nums[left] = nums[right];
            while (left < right && nums[left] < pivot){
    			left++;
    		}
    		nums[right] = nums[left];
    	}//结束外层while
    	nums[left] = pivot;
    	return left;
    }

----------
#冒泡插入
---
	public void bSort(int[] nums){
    	for (int i = 0; i < nums.length; i++){
    		for (int j = 0; j < nums.length - 1 - i; j++){
    			if (nums[j] > nums[j + 1]){
    				swap(nums, j, j + 1);
    			}
    		}//内层for
    	}//外层for
    }

    public void insertSort(int[] nums){
    	int len = nums.length;
    	for (int i = 0; i < len; i++){
    		int temp = nums[i];
    		int j = i - 1;
    		while (j >= 0 && nums[j] > temp){
    			nums[j + 1] = nums[j];
    			j--;
    		}
    		nums[j + 1] = temp;
    	}
    }
    
----------
#堆排序
----
	public void heapSort(int[] nums){
    	buildHeap(nums);
        for (int i=0; i<nums.length; i++){
    		swap(nums[0], nums[heapSize-1]);
    		heapSize--;
    		adjustHeap(nums,0);
    	}
    }
    public void buildHeap(int[] nums){
    	for (int i=heapSize/2-1;i>=0;i--){
    		adjustHeap(nums,i);
    	}
    }
    private void adjustHeap(int[] nums, int index){
    	int left = 2*index+1;
    	int right = left + 1;
    	int largest=index;
    	
    	if (left < heapSize && nums[left]>nums[index]){
    		largest=left;
    	} 
    	if (right <heapSize && nums[right] > nums[largest]){
    		largest = right;
    	}
    	if (largest != index){
    		swap(nums[index],nums[largest]);
    		adjustHeap(nums,largest);
    	}