# Sem.Assignment.1
A C Program that Downloads a new version of a  webpage if anything has anything on it.
// library includes
#include <stdio.h>
#include <string.h> 
#include <stdlib.h>

// function to compare 2 files
void compareFiles(FILE *fp1, FILE *fp2, char sitename[100]) 
{ 
	char buff[64];

    // fetching character of two file 
    // in two variable ch1 and ch2 
    char ch1 = getc(fp1); 
    char ch2 = getc(fp2); 
  
    // changes keeps track of number of changes 
    // pos keeps track of position of changes 
    // line keeps track of changes in line 
    int changes = 0, pos = 0, line = 1; 
  
    // iterate loop till end of file 
    while (ch1 != EOF && ch2 != EOF) 
    { 
        pos++; 
  
        // if both variable encounters new 
        // line then line variable is incremented 
        // and pos variable is set to 0 
        if (ch1 == '\n' && ch2 == '\n') 
        { 
            line++; 
            pos = 0; 
        } 
  
        // if fetched data is not equal then 
        if (ch1 != ch2) 
        { 
            sprintf(buff, "wget -O site_dump.html %s", sitename);
			system(buff);

            printf("Changes found, the site has been updated to the latest version.\n");
            break;
        }

        else
        {
        	printf("No changes found!!\n");
        	break;
        } 
  
        // fetching character until end of file 
        ch1 = getc(fp1); 
        ch2 = getc(fp2); 
    } 
} 

int main(int argc, char const *argv[])
{
	char sitename[100];
	char buff[64];
	char todo;

	printf("Enter Y if you want to check and update changes for the previously entered website else enter N if you want to add a new wesite\n");
	scanf("%c", &todo);

	if(todo == 'Y' || todo == 'y')
	{

		printf("\n Enter the url of the previous website \n");
		scanf("%s", sitename);

		sprintf(buff, "wget -O site_dump_check.html %s", sitename);
		system(buff);

		// opening both file in read only mode 
	    FILE *fp1 = fopen("./site_dump.html", "r"); 
	    FILE *fp2 = fopen("./site_dump_check.html", "r"); 

		// if either of the files is empty
	    if (fp1 == NULL || fp2 == NULL) 
	    { 
	       printf("Pleae enter a website first by pressing N in the beginning\n"); 
	       exit(0); 
	    } 
	  
	  	// calling function to compare
	    compareFiles(fp1, fp2, sitename); 
	  
	    // closing both file 
	    fclose(fp1); 
	    fclose(fp2); 

	}

	else
	{

		printf("\n Enter the url of new website \n");
		scanf("%s", sitename);

		sprintf(buff, "wget -O site_dump.html %s", sitename);
		system(buff);

		printf("\n Thank you for adding new website \n");

	}

	return 0;
}
