```
/*#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <ctype.h>
#include <wchar.h>
#include <locale.h>


typedef unsigned set_data_elem;
enum {
    bits_per_char = 16u,
    bits_per_elem = sizeof(set_data_elem) * bits_per_char,
    datalen = (1u << bits_per_char) / bits_per_elem
};

typedef struct {
    set_data_elem data[datalen];
} set;

void set_clear(set *s) {
    memset(s->data, 0, sizeof(s->data));
}

void set_insert(set *s, int c) {
    s->data[c / bits_per_elem] |= 1u << (c % bits_per_elem);
}


bool set_in(const set *s, int c) {
    return (s->data[c / bits_per_elem] & (1u << (c % bits_per_elem))) != 0;
}


bool set_includes(const set *s1, const set *s2) {
    for (int i = 0; i != datalen; i++) {
        if ((s1->data[i] | s2->data[i]) != s1->data[i]) return false;
    }
    return true;
}



bool is_alpha(int c) { return isalpha(c); }

bool is_digit(int c) { return isdigit(c); }


int main() {
    setlocale(LC_ALL, "");
    freopen("in.txt", "r", stdin);

    set empty_set, uSet, consonants;
    set_clear(&empty_set);
    set_clear(&uSet);
    set_clear(&consonants);

    set_insert(&consonants, L'п');
    set_insert(&consonants, L'ф');
    set_insert(&consonants, L'к');
    set_insert(&consonants, L'т');
    set_insert(&consonants, L'ш');
    set_insert(&consonants, L'с');

    wchar_t buf;
    while (wscanf(L"%lc", &buf) > 0) {
        if(buf >= L'а' && buf <= L'я'){
            if(set_in(&consonants, buf)){
                set_insert(&uSet, buf);
            }
        }else {
            for (wchar_t ch = L'а'; ch != L'я'; ch++) {
                if (set_in(&uSet, ch)) {
                    wprintf(L"Yes, this consonant is '%lc'\n", ch);
                    return 0;
                }
            }
        }
    }

} */

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <stdbool.h>

#define VOWELS ( \
    1U << ('b' - 'a') | \
    1U << ('v' - 'a') | \
    1U << ('g' - 'a') | \
    1U << ('d' - 'a') | \
    1U << ('z' - 'a') | \
    1U << ('l' - 'a') | \
    1U << ('m' - 'a') | \
    1U << ('n' - 'a') | \
    1U << ('r' - 'a') \
)

enum State {
    CHAR_IN_WORLD,
    NO_CHAR_IN_WORLD,
};
enum Gap {
    BE,
    NOT_BE,
};

unsigned int char_to_set(int c);
bool process();

int main() {

    if (process() == 1) {
        printf("Yes");
    } else printf("No");

    return 0;
}

unsigned int char_to_set(int c) {//преобразование буквы в цифру
    c= tolower(c);
    if (c < 'a' || c > 'z') {
        return 0;
    } else {
        return 1u << (c - 'a');
    }
}

bool process () {
    enum State state = CHAR_IN_WORLD;
    enum Gap gap = NOT_BE;
    unsigned int set = 0;
    
    int k = 0;

    for (char c = getchar(); c != '\n'; c = getchar())
    {

        switch (state)
        {
        case CHAR_IN_WORLD:
            if (isalpha(c) != 0) {
                c=tolower(c);
                if ((char_to_set(c) & VOWELS)!=0) {
                    set=char_to_set(c);
                    gap = NOT_BE;
                } 
                else {
                    state = NO_CHAR_IN_WORLD;
                }
            }
            else {
                gap = BE;
                k ++;
            }   
            break;        
        case NO_CHAR_IN_WORLD:
            if (isalpha(c) != 0) {
                gap = NOT_BE;    
            }
            else {
                state = CHAR_IN_WORLD;
                gap = BE;
            }
            break;
        }
       
    }  
     
        if (gap == NOT_BE && state == CHAR_IN_WORLD) {
            k++;
        }
        if ( k > 0 ) {
            return 1;
        } else {
            return 0;
        }
    return 0;
}
```
