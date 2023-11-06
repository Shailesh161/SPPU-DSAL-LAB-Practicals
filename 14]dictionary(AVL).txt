#include <iostream>
#include <string>
#include <cstring>
#include <cstdlib>
using namespace std;
int op;
class node
{
    public:
    node *left;
    char word[50],mean[50];
    node *right;
};
class BT
{
    public:
    node *root;
    BT()
    {
        root=NULL;
    }
    void create();
    node* insert(node *,node *);
    void inorder(node *);
    void preorder(node *);
    void postorder(node *);
    node *findsmallest(node *);
    node *findlargest(node *);
    void search(node *, char []);
    void modify(node *, char []);
    node *dlt(node * ,char []);
    node *FindMin(node * );
};
void BT::create()
{
    int op;
    node *temp;
    do
    {
        temp=new node;
        cout<<"Enter A word ";
        cin>>temp->word;
        cout<<"Enter A Meaning : ";
        cin>>temp->mean;
     temp->left=temp->right=NULL;
     if(root==NULL)
     {
         root=temp;
     }
     else
     {
         root=insert(root,temp);
     }
     cout<<"Want to insert again : ";
     cin>>op;
    }while(op==1);
}
node* BT::insert(node *root,node *temp)
{     int l;
int l1=strlen(temp->word);
int l2=strlen(root->word);
if(l1<l2)
{
    l=l1;
}
else
{
    l=l2;
}
for(int i=0;i<l;i++)
{
    if(temp->word[i]<root->word[i])
    {
        if(root->left==NULL)
        {
            root->left=temp;
            return root;
        }
        else
        {
            root->left=insert(root->left,temp);
            return root;
        }
    }
    else if(temp->word[i]>root->word[i])
    {
        if(root->right==NULL)
        {
            root->right=temp;
            return root;
        }
        else
        {
            root->right=insert(root->right,temp);
            return root;
        }
    }
}
return root;
}
void BT::inorder(node *temp)
{
    if(temp!=NULL)
    {
        inorder(temp->left);
        cout<<temp->word<<" "<<temp->mean;
        inorder(temp->right);
    }
}
void BT::preorder(node *temp)
{
    if(temp!=NULL)
    {
        cout<<temp->word<<" "<<temp->mean;
        preorder(temp->left);
        preorder(temp->right);
    }
}
void BT::postorder(node *temp)
{
    if(temp!=NULL)
    {
                     postorder(temp->left);
                     postorder(temp->right);
                     cout<<temp->word<<" "<<temp->mean;
    }
}
node* BT::findsmallest(node *root)
{
    if(root->left==NULL)
    {
        cout<<"\n\n Smallest Node:"<<root->word;
    }
    else
    {
        findsmallest(root->left);
    }
}
node* BT::findlargest(node *root)
{
    if(root->right==NULL)
    {
        cout<<"\n\n Largest Node:"<<root->word;
    }
    else
    {
    findlargest(root->right);
    }
}
void BT::search(node *temp , char src[])
{
if(temp != NULL)
{
 if((strcmp(temp->word , src)) == 0)
 {
    cout<<"\n Word Found ";
    cout<<"\n Word : "<<temp->word;
    cout<<"\n Meaning : "<<temp->mean;
 }
 else if((strcmp(temp->word , src )) != 0)
 {
     if((strcmp(temp->word , src)) < 0)
     {
         search(temp->right , src);

     }
     else if((strcmp(temp->word , src)) > 0)
     {
         search(temp->left , src);
     }
  }
}
else
    cout<<"\n Word Not Found ";
}
void BT::modify(node *temp , char src[])
{
    if(temp != NULL)
    {
     if((strcmp(temp->word , src)) == 0)
     {
        cout<<"\n Word Found ";
        cout<<"\n Enter New Meaning Of Word "<<temp->word;
        cin>>temp->mean;
     }
     else if((strcmp(temp->word , src )) != 0)
     {
         if((strcmp(temp->word , src)) < 0)
         {
             modify(temp->right , src);

         }
         else if((strcmp(temp->word , src)) > 0)
         {
             modify(temp->left , src);
         }
      }
    }
    else
        cout<<"\n Word Not Found ";
}
node* BT::dlt(node *root , char src[])
{
    if(root != NULL)
    {
       if((strcmp(root->word , src)) > 0)
       {
            root->left = dlt(root->left , src);
       }
       else if((strcmp(root->word , src)) < 0)
        {
            root->right = dlt(root->right , src);
        }
       else
       {
           if(root == NULL && root->right == NULL)
           {
             delete(root);
             root = NULL;
           }
           else if(root->left == NULL)
           {
               node *temp = root;
               root = root->right;
               delete(temp);
           }
           else if(root->right == NULL)
           {
               node *temp = root;
               root = root->left;
               delete(temp);
           }
           else
           {
               node *temp =  FindMin(root->right);
               strcpy(root->word , temp->word);
               strcpy(root->mean , temp->mean);
               root->right = dlt(root->right , temp->word);
           }
       }
    }
    return root;
}
node* BT:: FindMin(node* root)
{
  while(root->left != NULL) root = root->left;
  return root;
}
int main()
{
        BT b;
        int op;
        char src[100];
        while(1)
        {
         cout<<"\n ";
         cout<<"\n1.Insert Binary Search Tree ";
         cout<<"\n2.Display Inorder, Preorder and Postorder ";
         cout<<"\n3.Search The Word ";
         cout<<"\n4.Modify The Meaning Of Word ";
         cout<<"\n5.Delete Word From Dictionary ";
         cout<<"\n6.Largest and Smallest Node ";
         cout<<"\n7.Exit";
         cout<<"\nEnter your choice:";
         cin>>op;
         switch(op)
         {
         case 1:
            b.create();
            break;
         case 2:
            cout<<"\n Inorder : ";
            b.inorder(b.root);
            cout<<"\n Preorder : ";
            b.preorder(b.root);
            cout<<"\n Postorder : ";
            b.postorder(b.root);
            break;
         case 3:
             cout<<"\n Enter The Word Want To Search : ";
             cin>>src;
             b.search(b.root , src);
             break;
         case 4:
             cout<<"\n Enter The Word Want To Modify ";
             cin>>src;
             b.modify(b.root , src);
             break;
         case 5:
             cout<<"\n Enter The Word Want To Delete ";
             cin>>src;
             b.dlt(b.root , src);
             break;
         case 6:
             b.findsmallest(b.root);
             b.findlargest(b.root);
             break;
         case 7:
             exit(0);
             break;
         default :
             cout<<"\n Invalid Option ";
             break;
        }
    }
}


