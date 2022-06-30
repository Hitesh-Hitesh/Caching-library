#include <bits/stdc++.h>
using namespace std;

struct Node
{
    int key, value, count;
    Node *next;
    Node *prev;
    Node(int k, int v)
    {
        key = k;
        value = v;
        count = 1;
    }
};

struct List
{
    int size;
    Node *head;
    Node *tail;
    List()
    {
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head->next = tail;
        tail->prev = head;
        head->prev = NULL;
        tail->next = NULL;
        size = 0;
    }

    void addNode(Node *new_Node)
    {
        Node *temp = head->next;
        new_Node->next = temp;
        new_Node->prev = head;
        head->next = new_Node;
        temp->prev = new_Node;
        size++;
    }

    void deleteNode(Node *dNode)
    {
        Node *dNode_back = dNode->prev;
        Node *dNode_front = dNode->next;
        dNode_back->next = dNode_front;
        dNode_front->prev = dNode_back;
        size--;
    }
};

class LFUCache
{
    map<int, Node *> keyNode;
    map<int, List *> freqListMap;
    int maxSizeCache;
    int minFreq;
    int curSize;

public:
    LFUCache(int capacity)
    {
        maxSizeCache = capacity;
        minFreq = 0;
        curSize = 0;
    }

    void updateFreqListMap(Node *Node)
    {
        keyNode.erase(Node->key);
        freqListMap[Node->count]->deleteNode(Node); // Deleting that node from frequency list whose frequency is updating
        if (Node->count == minFreq && freqListMap[Node->count]->size == 0)
        {
            minFreq++; // when the sub-list of min_freq becomes empty, we increment the min_freq by 1
        }

        List *nextHigherFreqList = new List();
        if (freqListMap.find(Node->count + 1) != freqListMap.end())
        {
            nextHigherFreqList = freqListMap[Node->count + 1]; // if sub-list of +1 freq exists, then make new temp list = old list
        }
        Node->count += 1;
        nextHigherFreqList->addNode(Node);
        freqListMap[Node->count] = nextHigherFreqList;
        keyNode[Node->key] = Node;
    } // O(const)

    void get(int key)
    {
        if (keyNode.find(key) != keyNode.end())
        {
            Node *node = keyNode[key];
            int val = node->value;
            updateFreqListMap(node);
            cout << "Key : " << key << " "
                 << "Value: " << val << endl;
        }
        else
            cout << "Key : " << key
                 << " does not exist in the cache!! " << endl;
        cout << endl;
    }

    void put(int key, int value)
    {
        if (maxSizeCache == 0)
            return;
        if (keyNode.find(key) != keyNode.end())
        {
            Node *node = keyNode[key];
            node->value = value;
            updateFreqListMap(node);
            cout << "Added -> "
                 << "Key : " << key << "   "
                 << "Value : " << value << endl;
        }
        else
        {
            if (curSize == maxSizeCache)
            {
                cout << "Cache already full, will remove element from least frequency list using LRU and then insert the new element. " << endl;
                List *list = freqListMap[minFreq];
                cout << "Removing -> "
                     << "Key : " << freqListMap[minFreq]->tail->prev->key << "  "
                     << "Value : " << freqListMap[minFreq]->tail->prev->value << endl;
                keyNode.erase(list->tail->prev->key);
                freqListMap[minFreq]->deleteNode(list->tail->prev);
                curSize--;
            }
            curSize++;
            minFreq = 1;
            List *listFreq = new List();                        // creating new list to insert element into
            if (freqListMap.find(minFreq) != freqListMap.end()) // checking if list exits of freq=1
            {
                listFreq = freqListMap[minFreq]; // if found, then we equate temporary list to be same as old freq-1 list
            }
            Node *node = new Node(key, value);
            cout << "Added -> "
                 << "Key : " << key << "   "
                 << "Value : " << value << endl; // adding the element into the temporary list
            listFreq->addNode(node);
            keyNode[key] = node;
            freqListMap[minFreq] = listFreq; // updating new temporary list into the list map
        }
    }

    void TraverseDLL()
    {
        if (curSize == 0)
        {
            cout << "Cache is empty!! " << endl;
        }
        else
        {
            cout << "Traversing each frequency in MRU order " << endl;
            for (auto i : freqListMap)
            {
                List *temp = i.second;
                cout << "Frequency : " << i.first << endl;
                Node *n = temp->head->next;
                if (n != temp->tail)
                {
                    while (n != temp->tail)
                    {
                        cout << "Key : " << n->key << " "
                             << "Value : " << n->value;
                        cout << endl;
                        n = n->next;
                    }
                }
                else
                {
                    cout << "Empty! " << endl;
                }
                cout << endl;
            }
        }
        cout << endl;
    }

    void deleteKey(int key)
    {
        if (keyNode.find(key) != keyNode.end())
        {
            Node *node = keyNode[key];
            int node_count = node->count;
            freqListMap[node_count]->deleteNode(node);
            keyNode.erase(key);
            curSize--;
        }
        else
        {
            cout << "Key doesn't exist in the cache!! " << endl;
        }
    }

    void clearCache()
    {
        for (auto i : keyNode)
        {
            Node *node = i.second;
            int node_count = node->count;
            freqListMap[node_count]->deleteNode(node);
        }
        keyNode.clear();
        freqListMap.clear();
        curSize = 0;
    }
};

class LRUCache
{
public:
    class node
    {
    public:
        int key;
        int val;
        node *next;
        node *prev;
        node(int k, int v) // key, value pair we are storing
        {
            key = k;
            val = v;
        }
    };

    node *head = new node(-1, -1); // Intializing head and tail
    node *tail = new node(-1, -1);

    int capacity;
    unordered_map<int, node *> map;
    LRUCache(int cap)
    {
        capacity = cap;
        head->next = tail;
        tail->prev = head;
        head->prev = NULL;
        tail->next = NULL;
    }

    void addNode(node *new_node)
    {
        node *temp = head->next;
        new_node->next = temp;
        new_node->prev = head;
        head->next = new_node;
        temp->prev = new_node;
    }

    void deleteNode(node *dnode)
    {
        node *dnode_back = dnode->prev;
        node *dnode_front = dnode->next;
        dnode_back->next = dnode_front;
        dnode_front->prev = dnode_back;
        // We have not deleted the existence of dnode because it willl help to extra the value of node.
    }

    void put(int key, int value)
    {
        if (map.find(key) != map.end()) // Searching the key the map
        {
            node *existing_node = map[key]; // Storing the addresss of the node to be deleted using the node address
            map.erase(key);
            deleteNode(existing_node);
        }
        if (map.size() == capacity)
        {
            cout << "Cache already full, will remove element using LRU and then insert the element. " << endl;
            cout << "Removing -> "
                 << "Key : " << tail->prev->key << "   "
                 << "Value : " << tail->prev->val << endl;
            map.erase(tail->prev->key);
            deleteNode(tail->prev);
        }
        // My code line
        node *l1 = new node(key, value);
        addNode(l1);
        map[key] = head->next;
    }

    // Both get and put operations are O(1) because it is implemented using unodered map(which is implemented using hash table)

    void get(int key)
    {
        if (map.find(key) != map.end())
        { // Finding the key
            node *result_node = map[key];
            int res = result_node->val; // storing the result in the key
            map.erase(key);             // Because now address has changed
            deleteNode(result_node);
            addNode(result_node);
            map[key] = head->next;
            cout << "Value for the corresponding key : " << res << endl;
        }
        else
        {
            cout << "Key : " << key
                 << " does not exist in the cache!! " << endl;
        }
    }

    void deleteKey(int key)
    {
        if (map.find(key) != map.end())
        {
            node *l1 = map[key];
            cout << "Removing key : " << key << "  "
                 << "Value : " << l1->val << endl;
            deleteNode(l1);
            map.erase(key);
            return;
        }
        else
            cout << "Key does not exist!! " << endl;
    }

    void clearCache()
    {
        if (map.empty())
            cout << "Cache is already empty!! " << endl;
        else
        {
            for (auto i : map)
            {
                deleteNode(i.second);
            }
            map.clear();
        }
    }

    void TraverseMap()
    {
        if (map.empty())
            cout << "Map is Empty!! ";
        for (auto i : map)
        {
            cout << "Key : " << i.first << "    "
                 << "Value : " << i.second;
            cout << endl;
        }
        cout << endl;
    }

    void TraverseDLL()
    {
        cout << "Traversing the cache in MRU order " << endl;
        node *temp = head;
        if (temp->next != tail)
        {
            while (temp->next != tail)
            {
                cout << "Key : " << temp->next->key << "    ";
                cout << "Value : " << temp->next->val;
                temp = temp->next;
                cout << endl;
            }
        }
        else
        {
            cout << "Cache is Empty!! ";
        }
        cout << endl;
    }
};

int main()
{
    cout << "Welcome to Caching Library" << endl;
    cout << "Group members- Aryan Kapadia, Harsh Vardhan Gupta, Hitesh Garg" << endl
         << endl;

    cout << "Select f for LFU" << endl;
    cout << "Select r for LRU" << endl;

    char ch;
    cin >> ch;
    if (ch == 'r' || ch == 'R')
    {
        cout << "This is the LRU Cache" << endl;
        cout << "Enter the maximum possible size of the cache" << endl;
        int size;
        cin >> size;
        if (size <= 0)
        {
            cout << "Please insert a positive size for cache. " << endl;
            return 0;
        }
        LRUCache c1(size);
        cout << "Choose the action you want to perform" << endl;
        cout << "1: For putting a key,value pair" << endl;
        cout << "2: For getting back the value for the given key" << endl;
        cout << "3: For deleting an entry" << endl;
        cout << "4: For clearing the whole cache" << endl;
        cout << "5: For traversing the list" << endl;
        // cout<<"8 to go back to the main menu"<<endl;
        cout << "9: for exiting" << endl
             << endl;

        int in = 0;
        while (in != 9)
        {
            cout << "Choose an action : ";
            cin >> in;
            if (in == 1)
            {
                int key, value;
                cout << "Enter key: ";
                cin >> key;
                cout << "Enter value: ";
                cin >> value;
                cout << endl;
                c1.put(key, value);
            }
            else if (in == 2)
            {
                int key;
                cout << endl
                     << "Enter the key for which value is to be searched: ";
                cin >> key;
                c1.get(key);
            }
            else if (in == 3)
            {
                int key;
                cout << endl
                     << "Enter the key of the entry you want to delete: ";
                cin >> key;
                c1.deleteKey(key);
            }
            else if (in == 4)
            {
                cout << endl
                     << "Clearing cache..." << endl;
                c1.clearCache();
                cout << "cache cleared" << endl;
            }
            else if (in == 5)
            {
                cout << endl
                     << "Traversing cache..." << endl;
                c1.TraverseDLL();
            }
            // else if(in == 8)
            // {

            // }
            else if (in == 9)
            {
                exit;
            }
            else
            {
                cout << "Wrong Input" << endl;
            }
        }
    }
    else if (ch == 'F' || ch == 'f')
    {
        cout << "This is the LFU Cache" << endl;
        cout << "Enter the maximum possible size of the cache" << endl;
        int size;
        cin >> size;
        LFUCache c1(size);
        cout << "Choose the action you want to perform" << endl;
        cout << "1: For putting a key,value pair" << endl;
        cout << "2: For getting back the value for the given key" << endl;
        cout << "3: For deleting an entry" << endl;
        cout << "4: For clearing the whole cache" << endl;
        cout << "5: For traversing the list" << endl;

        // cout<<"8 to go back to the main menu"<<endl;
        cout << "9: for exiting" << endl
             << endl;

        int in = 0;
        while (in != 9)
        {
            cout << "Choose an action : ";
            cin >> in;
            if (in == 1)
            {
                int key, value;
                cout << "Enter key: ";
                cin >> key;
                cout << "Enter value: ";
                cin >> value;
                cout << endl;
                c1.put(key, value);
            }
            else if (in == 2)
            {
                int key;
                cout << endl
                     << "Enter the key for which value is to be searched: ";
                cin >> key;
                c1.get(key);
            }
            else if (in == 3)
            {
                int key;
                cout << endl
                     << "Enter the key of the entry you want to delete: ";
                cin >> key;
                c1.deleteKey(key);
            }
            else if (in == 4)
            {
                cout << endl
                     << "Clearing cache..." << endl;
                c1.clearCache();
                cout << "Cache cleared" << endl;
            }
            else if (in == 5)
            {
                cout << endl
                     << "Traversing cache..." << endl;
                c1.TraverseDLL();
            }
            // else if(in == 8)
            // {

            // }
            else if (in == 9)
            {
                exit;
            }
            else
            {
                cout << "Wrong input" << endl;
            }
        }
    }
    else
    {
        cout << "Incorrect input! " << endl;
    }
}

