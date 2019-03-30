# Revisi-TDL

## Source Code

```c
#include<stdio.h>
#include<stdlib.h>

typedef struct test{
	int col;
	int line;
	struct test *next;
}data;

data *head;
int matrix[401][401];

void init(){
	head=NULL;
}

void push(int i, int j){
	data *temp;
	temp=(data*)malloc(sizeof(data));
	temp->line=i;
	temp->col=j;
	temp->next=NULL;
	if(head==NULL){
		head=temp;
	}
	else{
		temp->next=head;
		head=temp;
	}
}

void pop(){
	data *temp;
	temp=head;
	if(head==NULL){
		return;
	}
	else{
		if(head->next!=NULL){
			head=head->next;
		}
		else{
			head=NULL;
		}
		free(temp);
	}
}

int main()
{
	init();
	int n, count=0,kol,bar;
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			scanf("%d", &matrix[i][j]);
		}
	}
	if(matrix[n-1][0]==0 || matrix[0][n-1]==0){
		printf("Buntu yaa Thanus\n");
	}
	else{
		push(n-1,0);
		bar=head->line;
		kol=head->col;
		while(head!=NULL){
			if(bar==0 && kol==n-1){
				count++;
				break;
			}
			else{
				if(matrix[bar-1][kol]==1 && bar>=0){
					push(bar-1,kol);
					matrix[bar][kol]=0;
				}
				if(matrix[bar][kol+1]==1 && kol<=n-1){
					push(bar,kol+1);
					matrix[bar][kol]=0;
				}
				if(matrix[bar+1][kol]==1 && bar<=n-1){
					push(bar+1,kol);
					matrix[bar][kol]=0;
				}
				if(matrix[bar][kol-1] && kol>=0){
					push(bar,kol-1);
					matrix[bar][kol]=0;
				}
			}
			bar=head->line;
			kol=head->col;
			pop();
		}
		if(count==0) printf("Buntu yaa Thanus\n");
		else printf("Ada jalan yaa Thanus\n");
	}
	
	return 0;
}
```

Pada dasarnya, program ini sama saja dengan problem "Rat in Maze". Jalan yang dapat dilalui ditandai dengan angka "1", sedangkan yang tidak dapat dilalui ditandai dengan angka "0". Jalur berawal dari sudut kiri paling bawah dan berakhir pada sudut kanan paling atas. Untuk jalur yang dilalui bebas selagi jalur tersebut masih terhubung dengan angka "1" dari sudut kiri paling bawah ke sudut kanan paling atas. Dalam program ini, dilakukan dengan metode struktur data stack.

#### Inisialisasi dan Pendeklarasian Struct
Pada bagian ini, dideklarasikan struct dengan isi integer yang menandakan untuk baris dan kolom. Kemudian, dideklarasikan juga matriks yang akan digunakan sebagai jalur "Rat in Maze". Diinisialisasikan ```head```bernilai NULL.
```c
#include<stdio.h>
#include<stdlib.h>

typedef struct test{
	int col;
	int line;
	struct test *next;
}data;

data *head;
int matrix[401][401];

void init(){
	head=NULL;
}
```

#### Fungsi Push
Bagian ini untuk push koordinat matriks ke dalam stack. Nantinya, ini digunakan untuk menentukan pergerakan dalam mencari jalan keluar dari problem yang diberikan. Apakah masih dapat lanjut mencari jalan atau tidak, ditentukan dari banyaknya koordinat matriks yang tertampung di dalam stack.
```c
void push(int i, int j){
	data *temp;
	temp=(data*)malloc(sizeof(data));
	temp->line=i;
	temp->col=j;
	temp->next=NULL;
	if(head==NULL){
		head=temp;
	}
	else{
		temp->next=head;
		head=temp;
	}
}
```

#### Fungsi Pop
Bagian ini untuk pop head yang ada dalam stack. Head yang ada di dalam stack akan di pop tanpa diambil nilainya dan nantinya akan menentukan juga untuk pengecekan jalur dalam matriks.
```c
void pop(){
	data *temp;
	temp=head;
	if(head==NULL){
		return;
	}
	else{
		if(head->next!=NULL){
			head=head->next;
		}
		else{
			head=NULL;
		}
		free(temp);
	}
}
```

#### Main Program
Bagian ini merupakan main program yang akan dijalankan. Dilakukan pendeklarasian variabel yang akan digunakan. Setelah dideklrasasikan, program meminta untuk input nilai ```n``` sebagai ordo matriks yang akan digunakan. Setelahnya, program meminta untuk input angka "1" atau "0" ke dalam matriks sebagai jalur dalam matriks.
```c
int main()
{
	init();
	int n, count=0,kol,bar;
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			scanf("%d", &matrix[i][j]);
		}
	}
 ```
  
Setelah nilai matriks diinputkan, akan di cek apakah ada jalur yang dapat dilalui dari sudut kiri paling bawah menuju sudut kanan paling atas. Jika sudut kiri paling bawah ataupun sudut kanan paling atas bernilai "0", maka output ```Buntu yaa Thanus``` akan ditampilkan dan program akan langsung selesai. Tetapi jika tidak, akan dilakukan pengecekan jalur. Koordinat sudut kiri paling bawah di push ke dalam stack sebagai head, dimana pencarian jalur dimulai dari sini. Looping untuk mencari jalur dilakukan, selama nilai ```head``` bukan NULL. Jika sudah mencapai koordinat tujuan (sudut kanan paling atas), maka nilai ```count``` akan bertambah dan looping dihentikan. Pengecekan dilakukan ke arah atas, kanan, bawah, dan kiri. Jika sudah dicek, koordinat yang sudah di cek nilainya akan menjadi "0" agar tidak dicek ulang dan menjadi infinity loop. Koordinat yang bernilai "1" di push ke dalam stack agar nilai stack bertambah. Kemudian, pengecekan lanjut ke koordinat selanjutnya, dan ```head``` pada stack di pop.
 ```c
  if(matrix[n-1][0]==0 || matrix[0][n-1]==0){
		printf("Buntu yaa Thanus\n");
	}
	else{
		push(n-1,0);
		bar=head->line;
		kol=head->col;
		while(head!=NULL){
			if(bar==0 && kol==n-1){
				count++;
				break;
			}
			else{
				if(matrix[bar-1][kol]==1 && bar>=0){
					push(bar-1,kol);
					matrix[bar][kol]=0;
				}
				if(matrix[bar][kol+1]==1 && kol<=n-1){
					push(bar,kol+1);
					matrix[bar][kol]=0;
				}
				if(matrix[bar+1][kol]==1 && bar<=n-1){
					push(bar+1,kol);
					matrix[bar][kol]=0;
				}
				if(matrix[bar][kol-1] && kol>=0){
					push(bar,kol-1);
					matrix[bar][kol]=0;
				}
			}
			bar=head->line;
			kol=head->col;
			pop();
		}
```

Jika sudah keluar looping, dicek nilai variabel ```count```. Jika nilainya "0", output ```Buntu yaa Thanus``` akan ditampilkan. Namun jika tidak, output ```Ada jalan yaa Thanus``` akan ditampilkan sebagai tanda ditemukannya jalur. Kemudian, program selesai.
```c
if(count==0) printf("Buntu yaa Thanus\n");
		else printf("Ada jalan yaa Thanus\n");
	}
	
	return 0;
}
```
