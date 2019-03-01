simple tokenize example of simple calculate

    
    #include<stdio.h>
    
    enum tokentype
    {
        OP, NUM, LP, RP
    };
    typedef struct token {
        int tokentype;
        char val[16];
        struct token* prev;
        struct token* next;
    } token;
    
    token* newtoken()
    {
        token* tokennew = malloc(sizeof(token));
        tokennew->next = NULL;
        tokennew->prev = NULL;
        tokennew->val[0] = '\0';
        return tokennew;
    }
    
    
    void tokenmoveon(token** tokencurr)
    {
        (*tokencurr)->next = newtoken();
        (*tokencurr)->next->next = NULL;
        (*tokencurr)->next->prev = (*tokencurr);
        (*tokencurr) = (*tokencurr)->next;
    }
    
    void printtoknetype(int type)
    {
        switch (type)
        {
        case OP:
            printf("OP");
            break;
        case NUM:
            printf("NUM");
            break;
        case LP:
            printf("LP");
            break;
        case RP:
            printf("RP");
            break;
        default:
            break;
        }
    }
    
    void printtoken(token* thead)
    {
        token* p;
        p = thead;
        printf("tokne:\n");
        while (p != NULL)
        {
            printtoknetype(p->tokentype);
            printf(":[%s] ", p->val);
            p = p->next;
        }
    }
    
    void tokenone(token** tokencurr, int tokentype, char op)
    {
        (*tokencurr)->tokentype = tokentype;
        (*tokencurr)->val[0] = op;
        (*tokencurr)->val[1] = '\0';
        tokenmoveon(tokencurr);
    }
    
    token* tokenize(char* str)
    {
        if (!*str) printf("no token\n");
        token* thead;
        token* tokencurr = newtoken();
        thead = tokencurr;
    
        int i, idx = 0;
        char word[16];
        for (i = 0; i < strlen(str); i++) {
            char next = str[i + 1];
            switch (str[i])
            {
            case ' ':
                continue;
            case '\0':
                break;
            case '+':  //after + will be num (after num will be a op) or (
                tokenone(&tokencurr, OP, '+');
                continue;
            case '-':
                tokenone(&tokencurr, OP, '-');
                continue;
            case 'x':
                tokenone(&tokencurr, OP, 'x');
                continue;
            case '/':
                tokenone(&tokencurr, OP, '/');
                continue;
            case '(':
                tokenone(&tokencurr, LP, '(');
                continue;
            case ')':
                tokenone(&tokencurr, RP, ')');
                continue;
            default:
                word[idx++] = str[i]; // digit num
                if (!isdigit(next))  // finish num
                {
                    word[idx] = '\0';
                    strcpy(tokencurr->val, word);
                    idx = 0;
                    tokencurr->tokentype = NUM;
                    tokenmoveon(&tokencurr);
                }
                continue;
            }
        }
        //delete last token, is null token, because we tokenmoveon create token node first
        tokencurr = tokencurr->prev;
        free(tokencurr->next);
        tokencurr->next = NULL;
        return thead;
    
    }
    
    
    void getline(char* input, int inputsize) {
        int len = inputsize;
        int idx = 0;
        input[idx] = '\0';
        while (1)
        {
            char c = fgetc(stdin);
            if (c == EOF)
                break;
            if (c == '\n')
            {
                input[idx++] = '\0';
                break;
            }
            else {
                input[idx++] = c;
            }
            if (idx >= len - 1)
            {
                input[idx++] = '\0';
                break;
            }
        }
    }
    
    int main()
    {
        char input[128];
        getline(input, 128);
        printf("input: %s\n", input);
    
        token* tokenlist = tokenize(input);
        printtoken(tokenlist);
    
    
        return 0;
    }