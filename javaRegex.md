### regex in java 

# what is regex ?
A regular expression is a sequence of characters that construct a search patten . when you search for data in a text , you can use this search pattern to describe what you are looking for ....
![{894C2BF2-A0BF-4990-B63F-A281AAB47B5B}](https://github.com/user-attachments/assets/eb0563e2-7b4a-42c2-87cb-be94b07d58bb)

# what is java regex ?
The java Regex is an API which is used to define a pattern for searching or manipulating Strings . It is widely used to define the constraint on String such as password and Email validation . 

#matcher class

![{E3DEEB10-C774-4A39-8453-98D0F9BC5DE1}](https://github.com/user-attachments/assets/0911b57c-1902-4eb1-8f88-6661992d1e82)

# Pattern class 
Patten class is the complied version of the regular expressoin that is used to match the patten inside string . 
![{07380F74-8AA9-4AB3-9946-A7C6E9D04FA6}](https://github.com/user-attachments/assets/04d89d20-b8e1-4490-8e8f-f16372bad214)

# some examples 
1. 10. Find All Digits in a String
       example - String  str="The order number is 1234 and the code is 5678";

solution -> 
'''java 
    public static void main(String[] args) {

       
        String  str="The order number is 1234 and the code is 5678";
        Pattern p = Pattern.compile("[0-9]+");
        Matcher m = p.matcher(str);
        while(m.find()){
            System.out.println(m.group());
        }
    }
'''










