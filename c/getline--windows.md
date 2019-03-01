console get line input, windows test

    #include<stdio.h>
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
    	return 0;
    }