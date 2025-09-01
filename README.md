#include<iostream>
#include<vector>

using namespace std;

struct Node {
    int val;
    Node *left, *right;
    Node(int x) : val(x), left(NULL), right(NULL) {}
};

Node* findMin(Node* root) {
    while (root && root->left != NULL)
        root = root->left;
    return root;
}

Node* deleteNode(Node* root, int key) {
    if (root == NULL) return root;

    if (key < root->val) {
        root->left = deleteNode(root->left, key);
    } 
    else if (key > root->val) {
        root->right = deleteNode(root->right, key);
    } 
    else {
        if (root->left == NULL && root->right == NULL) {
            delete root;
            return NULL;
        }
        else if (root->left == NULL) {
            Node* temp = root->right;
            delete root;
            return temp;
        } 
        else if (root->right == NULL) {
            Node* temp = root->left;
            delete root;
            return temp;
        }
        else {
            Node* succ = findMin(root->right);
            root->val = succ->val;
            root->right = deleteNode(root->right, succ->val);
        }
    }
    return root;
}


Node* insert(Node* root, int key) {
    if (key == -1) return root;
    if (root == NULL) {
        return new Node(key);   // place found
    }
    if (key < root->val) {
        root->left = insert(root->left, key);
    } else if (key > root->val) {
        root->right = insert(root->right, key);
    }
    return root;
}

void getInorderTraversal(Node* root, vector<int> &ans) {
    if (root == NULL) return;
    getInorderTraversal(root->left, ans);
    ans.push_back(root->val);
    getInorderTraversal(root->right, ans);
}


Node* getBinaryTree(vector<int> &keys) {
    Node* root = NULL;
    for (int key : keys) {
        root = insert(root, key);
    }
    return root;
}

int main(){
    cin.tie(0); cout.tie(0);
    ios_base::sync_with_stdio(false);

    int t;
    cin >> t;
    while(t--){
        int n;
        cin>>n;
        vector<int>arr(n);
        for(int i=0;i<n;i++) cin>>arr[i];
        Node* tree = getBinaryTree(arr);
        int key;
        cin>>key;
        tree = deleteNode(tree, key);
        vector<int> ans;
        getInorderTraversal(tree, ans);
        for(auto v:ans){
            cout<<v<<" ";
        }
        cout<<"\n";
    }
    return 0;
}
