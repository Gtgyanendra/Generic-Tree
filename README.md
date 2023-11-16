# Generic-Tree
import java.util.*;
public class Main
{
    
    public static class Node{
        int data;
        ArrayList<Node> children = new ArrayList<>();
        
        Node(int data){
            this.data=data;
        }
        Node(){
            
        }
    }
    
    private static class Pair{
        Node node;
        int level;
        
        Pair(Node node,int level){
            this.node=node;
            this.level=level;
        }
        
    }
    
    public static Node constructor(int[] arr){
       
        Node root=null;
        Stack<Node> st=new Stack<>();
        
        
        int i=0;
        while(i<arr.length){
            
            if(arr[i]==-1){
                st.pop();
            }
            else{
                Node t=new Node();
                t.data=arr[i];
                
                if(st.size()>0)
                   st.peek().children.add(t);
                else root=t;
                
                st.push(t);
            }
            i++;
        }
        
        return root;
    }
    
    public static void display1(Node node){
        
        String str=node.data +" -> ";
        
        for(Node val:node.children){
            str+= val.data+",";
        }
        
        System.out.println(str);
        
        for(Node val:node.children){
            display1(val);
        }
        
    }
    public static void display2(Node node){
        System.out.print(node.data+"- ");
        
        for(Node child:node.children){
            System.out.print(child.data+", ");
        }
        
        System.out.println();
        
        for(Node child:node.children){
            display2(child);
        }
    }
    
    public static int size(Node root){
        
        int ans=0;
        
        for(Node child:root.children){
            int cs=size(child);
            ans=ans+cs;
        }
        ans=ans+1;
        
        return ans;
    }
    
    public static int maximun(Node node){  
        
        int max=Integer.MIN_VALUE;
        
        for(Node child:node.children){
            int cm=maximun(child);
            
            max=Math.max(max,cm);
        }
        
        max=Math.max(node.data,max);
        return max;
    }
    
    public static void traversal(Node node){
        
        System.out.println("Node pre "+node.data);
        
        for(Node child:node.children){
            System.out.println("Edge pre- "+node.data+"- "+child.data);
            
            traversal(child);
            
            System.out.println("Edge post- "+node.data+"- "+child.data);
        }
        
        
        System.out.println("Node post- "+node.data);
    }
    public static void print_Leve1(Node root){
        Queue<Node> qu=new ArrayDeque<>();
        qu.add(root);
        
        while(qu.size()>0){
            root=qu.remove();
            System.out.print(root.data+" ");
            
            for(Node child:root.children){
                qu.add(child);
            }
        }
        
        
    }
    
    public static void print_Leve2_approch1_using_two_queue(Node root){
        
        
        Queue<Node> parent=new ArrayDeque<>();
        Queue<Node> child=new ArrayDeque<>();
        
        parent.add(root);
        
        while(parent.size()>0){
            //1. Remove
            root=parent.remove();
            
            //2. Print
            System.out.print(root.data+" ");
            
            //3 Add
            for(Node val:root.children){
                child.add(val);
            }
            
            if(parent.size()==0){
                System.out.println();
                parent=child;
                
                child=new ArrayDeque<>();
            }
        }
        
     
    }
    
    public static void print_Leve2_approch2_using_one_queue_null_wala(Node node){
          Queue<Node> qu=new ArrayDeque<>();
        qu.add(node);
        
        qu.add(new Node(-1));
        
        while(qu.size()>0){
            node = qu.remove();
            
            if(node.data==-1){
                if(qu.size()>0){
                    System.out.println();
                    qu.add(new Node(-1));
                }
            }
            else{
                
                System.out.print(node.data+" ");
                
                for(Node child:node.children){
                    qu.add(child);
                }
                
            }
        }
    }
    
    public static void print_Leve2_approch3_using_one_queue_size_wala(Node node){
        Queue<Node> qu=new ArrayDeque<>();
        qu.add(node);
        
        while(qu.size()>0){
            
            int child_size=qu.size();
            
            for(int i=0;i<child_size;i++){
                //1.Remove
                node=qu.remove();
                //2.print
                System.out.print(node.data+" ");
                
                //3. Add
                
                for(Node child:node.children){
                    qu.add(child);
                }
            }
            
            System.out.println();
        }
    }
    
    public static void print_Leve2_approch4_using_class_pair_wala(Node node){
        
        Queue<Pair> qu=new ArrayDeque<>();
        
        qu.add(new Pair(node,1));
        int level=1;
        
        while(qu.size()>0){
            // Remove
            Pair p=qu.remove();
        
           if(p.level>level){
               level=p.level;
               System.out.println();
           }
           //print
           
           System.out.print(p.node.data+" ");
           
            //Add
           for(Node child:p.node.children){
               Pair cp=new Pair(child,p.level+1);
               qu.add(cp);
           }
           
        }
    }
    
    public static void print_zig_zag(Node node){
        
        Stack<Node> child=new Stack<>();
        Stack<Node> parent=new Stack<>();
        
        parent.push(node);
        
        int level=1;
        
        while(parent.size()>0){
            // 1. Remove
            node=parent.pop();
            
             // 2. Print
            System.out.print(node.data+" ");
            
            
            //3. Add 
            
            // odd level
            if(level%2==1){
                
                for(int i=0;i<node.children.size();i++){
                    Node temp=node.children.get(i);
                    child.push(temp);
                }
            }
            else{
                
                for(int i=node.children.size()-1;i>=0;i--){
                    Node temp=node.children.get(i);
                    child.push(temp);
                }
            }
            
            
            // If size is zero then put all the element of child to parent stack and clean child stack
            if(parent.size()==0){
                
                System.out.println();
                
                parent=child;
                
                child=new Stack<>();
                
                level++;
                
            }
        }
        
    }
    
    public static void print_mirror(Node node){
        
        // Expect Node apne children ko reverse karna janta hai 
        
        for(Node child:node.children){
            print_mirror(child);
        }
        
        // Ab hum node ko node ke children ko reverse kar dege 
        
        Collections.reverse(node.children);
        
        // Total all reverse hogya hai
    }
    
    public static void remove_leaves(Node node){
        for(int i=node.children.size()-1;i>=0;i--){
            Node child=node.children.get(i);
            
            if(child.children.size()==0){
                node.children.remove(child);
            }
        }
        
        for(Node child:node.children){
            remove_leaves(child);
        }
    }
    
    public static boolean find_element_in_generic_tree(Node node,int target){
        if(node.data==target){
            return true;
        }
        
        for(Node child:node.children){
            boolean ca = find_element_in_generic_tree(child,target);
            if(ca){
                return true;
            }
        }
        
        return false;
    }
    
    public static ArrayList<Integer> Node_to_root_path(Node node, int target){
        
        if(node.data==target){
            ArrayList<Integer> list=new ArrayList<>();
            list.add(node.data);
            return list;
        }
        
        for(Node child: node.children){
            ArrayList<Integer> ca=Node_to_root_path(child,target);
            
            if(ca.size()>0){
                ca.add(node.data);
                return ca;
            }
        }
        
        return new ArrayList<Integer>();
        
    }
    
    public static int find_distance_between_two_nodes(Node root,int first,int second){
        ArrayList<Integer> first_node=Node_to_root_path(root,first);
        ArrayList<Integer> Second_node=Node_to_root_path(root,second);
        
        int temp=0;
        int i=first_node.size()-1, j=Second_node.size()-1;
        
        while(i>=0 && j>=0){
            
            if(temp==1){
                break;
            }
            if(first_node.get(i)==Second_node.get(j)){
                temp=1;
            }
            i--;
            j--;
        }
        
       return i+j;
    }
    
    public static boolean Are_smilar_tree(Node node1,Node node2){
         /*
	    1. First we have to check size of node if both are not equal then both tree not same.
	    2. If both tree are same, then chek all the children size is same if not then there shape is not mentain.
	    3.Above both codition is true then return true.
	    
	    */
        
        if(node1.children.size()!=node2.children.size()){
            return false;
        }
        
        for(int i=0;i<node1.children.size();i++){
            Node child1=node1.children.get(i);
            Node child2=node2.children.get(i);
            
            if(Are_smilar_tree(child1,child2)==false){
                return false;
            }
        }
        
        return true;
    
    }
   
    public static boolean Check_If_mirror_image(Node root1,Node root2){
        /*Case-1. First we check both root size not eual then return false;
         Case-2. If both size are equal then check all the children's of both root are not equal then  return false;
                  --> First root start in 0 th element.
                  --> Second root start in last node-1 -> means size()-1
                  Because we check root1 is mirror to root2, so thats why we itrate above type.
         Case-3. After check Above both condition then return trure */
        
        //1. Case -1
        
        if(root1.children.size()!=root2.children.size()){
            return false;
        }
        
        //2. Case-2
        
        for(int i=0;i<root1.children.size();i++){
            
            int j=root2.children.size()-1-i;
            
            Node child1= root1.children.get(i);
            Node child2= root2.children.get(j);
            
            if(Check_If_mirror_image(child1,child2)==false){
                return false;
            }
        }
        
        // Case -3
        
        return true;
        
    }
    
    public static boolean Are_Symmetric_tree(Node root){
        return Check_If_mirror_image(root,root);
    }
    
    static int height;
    static int max;
    static int min;
    static int size;
    
    public static void multi_solver(Node node,int depth){
        size++;
        min=Math.min(min,node.data);
        max=Math.max(max,node.data);
        height=Math.max(height,depth);
        
        for(Node child:node.children){
            multi_solver(child,depth +1);
        }
    }
    
    static int predecessor;
    static int successor;
    static int state;
    
    public static void pre_suc(Node node,int target){
        
       if(state==0){
           if(target==node.data){
               state = 1;
           }
           else{
               predecessor=node.data;
           }
       }
       else if(state==1){
           successor=node.data;
           state=2;
       }
        
        for(Node child:node .children){
            pre_suc(child,target);
        }
        
    }
    
    static int ceil;
    static int floor;
    
    public static void ceil_floor(Node node,int data){
        
        if(node.data>data){
            if(node.data<ceil){
                ceil=node.data;
            }
        }
        
        if(node.data<data){
            if(node.data>floor){
                floor=node.data;
            }
        }
        
        
        for(Node child:node.children){
            ceil_floor(child,data);
        }
    }
    
    
    // static int k=3;
    // public static int k_th_largets_value(Node node){
    //     if(k==0){
    //         return node.data;
    //     }
    //     k--;
        
    //     for(Node child:node.children){
    //         int ck= k_th_largets_value(child);
            
    //         for()
    //     }
        
    //     return 0;
    // }
    
    static int dai=0;
    
    public static int heighest_daimeter_of_between_two_nodes(Node node){
        int hi=-1;
        int shi=-1;
        
        // Main formula = max_First_child_height + max_Sceond_child_hight + 2 
        
        for(Node child:node.children){
            int ch=heighest_daimeter_of_between_two_nodes(child);
            
            if(ch>hi){
                //Swap of two value;
                shi=hi;
                hi=ch;
            }
            else if(ch>shi){
                shi=ch;
            }
        }
        
        int candidate= hi+shi+2;
        
        if(candidate > dai){
            dai=candidate;
        }
        
        hi+=1;  // Khud ki height kya hoti hai - height +1= Khud ki height
        
        return hi;
        
    }
    
    public static void print_pre_and_post_order_without_using_recursion(Node node){
        
        Stack<Pair> st=new Stack<>();
        st.push(new Pair(node,-1));
        
        // case1 if st.peek() l me jo level rhega ohh yad -1 hai to pri order me print kra dena hai.4 level ko increase kr dena hai.
        // case 2 if st.peek() me jo level ki value children.size ke equal  hai to use hame post order me print kra dena hai aur stack se pop kar dena hai.
        // case3 if st.peek().level ki value 0 to children.size()-1 tk hai to kch nai karna hai bs child ko pair bna ke level me -1 dal ke push kar dena hai aur hame peek ki level ki value increase kar dena hai.
        
        String pre="";
        String post="";
        
        while(st.size()>0){
            
            Pair top=st.peek();
            
            if(top.level==-1){
                // Case -1
                pre = pre + top.node.data + " ";
                top.level++;
            }
            else if(top.level == top.node.children.size()){
                // Case -2
                
                post += top.node.data + " ";
                st.pop();
            }
            else{
                // Case -3
                Pair temp=new Pair(top.node.children.get(top.level),-1);
                st.push(temp);
                
                top.level ++;
            }
        }
        
        System.out.println(pre);
        System.out.println(post);
        
        
    }
    
	public static void main(String[] args) {
	    
	   
	   // int[] arr={10,20,50,-1,60,-1,-1,30,70,-1,80,110,-1,120,-1,-1,90,-1,-1,40,100};
	   // int[] arr2={10,20,50,-1,60,-1,-1,30,70,-1,80,110,-1,120,-1,-1,90,-1,-1,40,100};
	    
	    int[] arr={10,20,-1,30,50,-1,60,-1,-1,40};
	    
	    Node root=constructor(arr);
	   // Node root1=constructor(arr2);
	    
        // display1(root);
        // display2(root);
        
        // System.out.println(size(root));
        
        //  System.out.println(maximun(root));
        
        // traversal(root);
        
        // print_Leve1(root);
        
        // print_zig_zag(root);
        
        // print_Leve2_approch1_using_two_queue(root);
        // print_Leve2_approch2_using_one_queue_null_wala(root);
        // print_Leve2_approch3_using_one_queue_size_wala(root);
        // print_Leve2_approch4_using_class_pair_wala(root);
        
        
        // print_mirror(root);
        // display1(root);
        
        // remove_leaves(root);
        // display(root);
        
        // System.out.println(find_element_in_generic_tree(root,10));
        
        // ArrayList<Integer> ans=Node_to_root_path(root,10);
        // for(int i=0;i<ans.size();i++){
        //     System.out.print(ans.get(i)+" ");
        // }
        
        // System.out.println(find_distance_between_two_nodes(root,70,120));
        
        // System.out.println(Are_smilar_tree(root,root1));
        
        
        // IMPT- Pending worjing after some time
        // Node root2=return_mirror(root1);
        // display1(root2);
        // System.out.println(Check_If_mirror_image(root,root2));
        
        // System.out.println(Are_Symmetric_tree(root));
        
        // size=0;
        // min=Integer.MAX_VALUE;
        // max=Integer.MIN_VALUE;
        // height=0;
        // multi_solver(root,0);
        // System.out.println("Size "+size);
        // System.out.println("Min "+min);
        // System.out.println(("Max "+max));
        // System.out.println("Height "+height);
        
        // predecessor=0;
        // successor=0;
        // state=0;
        // pre_suc(root,120);
        // System.out.println("predecessor "+predecessor);
        // System.out.println("successor "+successor);
        
        
        // ceil=Integer.MAX_VALUE;
        // floor=Integer.MIN_VALUE;
        // ceil_floor(root,90);
        // System.out.println("ceil "+ceil);
        // System.out.println("floor "+floor);
        
        //k_th_largets_value(root); // khud se solve karna hai same ceil and floor wala concept lagega
        
        //Maximum_sum_of_subtree // khud se karna hai isme bhi multi_solver jaise hi karna hai is question me return kch aur karna hai aur print kch aur karna hai
        
        // heighest_daimeter_of_between_two_nodes(root);
        // System.out.println(dai);
        
        print_pre_and_post_order_without_using_recursion(root);
        
	}
}
