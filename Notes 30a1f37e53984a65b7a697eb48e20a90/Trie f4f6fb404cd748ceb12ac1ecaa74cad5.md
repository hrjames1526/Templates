# Trie

```cpp
struct Trie {
    TrieNode *root;
    Trie() {
        root = new TrieNode();
    }
    void AddWord(const string &s) {
        int n = (int) s.size();
        TrieNode *p = root;
        for (int i = 0; i < n; ++i) {
            int c = s[i] - 'a';
            if (p->child[c] == NULL) p -> child[c] = new TrieNode();
            p = p->child[c];
            ++p->cnt;
        }
        ++p->endmark;
    }
    int CountPrefix(const string &s) {
        int n = (int) s.size();
        TrieNode *p = root;
        for (int i = 0; i < n; ++i) {
            int c = s[i] - 'a';
            if (p->child[c] == NULL) return 0;
            p = p->child[c];
        }
        return p->cnt;
    }

} trie;
```