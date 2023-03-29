#include<iostream>
#include<cmath>
#include<vector>

using namespace std;
class tupla{
    private:
    int x;
    int y;
    public:
    tupla(){
    x = 0;
    y = 0;
    }
    ~tupla(){}
    int get_x(){
        return x;    
    }
    int get_y(){
        return y;
    }
    tupla(int v,int n){
    x = v;
    y = n;
    }
    
    
};

float distancia(tupla a,tupla b){
    float cuadrado_x = pow((b.get_x()-a.get_x()),2);
    float cuadrado_y = pow(b.get_y()-a.get_y(),2);
    return sqrt(cuadrado_x+cuadrado_y);
 
}

template<typename T>
class Nodo{
private:
  T dato;
  Nodo* next;

public:

 Nodo(T d){
     dato = d;
     next =  NULL;
 }
 
 ~Nodo(){
 }
 
 T get_dato(){
     return dato;
 }
 
 void set_dato(T d){
     dato = d;
 }
 
 Nodo* get_next(){
     return next;
 }
 
 void set_next(Nodo* n){
     next = n;
 }
 
    
};

template<typename T>
class Lista{
private:
  Nodo<T>* ptr;
  int size;

public:

    Lista(){
        ptr = NULL;
        size = 0;
    }
  
    ~Lista(){
        Nodo<T>* temp = ptr;
        if(ptr != NULL){
            Nodo<T>* temp_next = ptr->get_next();
            while(temp_next != NULL){
                delete temp;
                temp = temp_next;
                temp_next = temp->get_next();
            }
            delete temp; //Borrar el último nodo
        }
    }
    
    void add(T d){
        Nodo<T>* nodo = new Nodo<T>(d);
        
        if(ptr == NULL){//La lista está vacía
            ptr  = nodo;
        }else{//La lista no está vacía
            Nodo<T>* temp =  ptr;
            while(temp->get_next() != NULL){
                temp = temp->get_next();
            }
            temp->set_next(nodo);
        }
        size++;
    }
    
    void insert(T d, int i){
        if(i<=size && i>=0 && ptr != NULL){
            Nodo<T>* nodo = new Nodo<T>(d);
            if(i == 0){
                nodo->set_next(ptr);
                ptr = nodo;
            }else{
                int j = 0;
                Nodo<T>* temp =  ptr;
                while(j<i-1){
                    temp = temp->get_next();
                    j++;
                }
                nodo->set_next(temp->get_next());
                temp->set_next(nodo);
            }
            size++;
        }else{//Si el índice es incorrecto o la lista está vacía, se añade al final
            add(d);
        }
        
    }
    
    void print(){
        Nodo<T>* temp =  ptr;
        while(temp != NULL){
            //cout<<temp->get_dato()<<","<<temp->get_next()<<"\t";
            cout<<temp->get_dato()<<"\t";
            temp = temp->get_next();
        }
        cout<<endl;
    
    }
    
    Nodo<T>* get(int i){
        if(i>=0 && i<size){
          int j=0;
          Nodo<T>* temp = ptr;
          while(j<i){
              temp = temp->get_next();
              j++;
          }
          return temp;
        }
        return NULL;
    }
    vector<float> distancias(tupla x){
        vector<float> v;
        Nodo<tupla>* temp = ptr;
        while(temp!=NULL){
            float d = distancia(temp->get_dato(),x);
            v.push_back(d);
            temp=temp->get_next();
        }
        return v;
    }
    
};

void shellSort(vector<float>v) {
    int n = v.size();
  for (int interval = n / 2; interval > 0; interval /= 2) {
    for (int i = interval; i < n; i += 1) {
      float temp = v[i];
      int j;
      for (j = i; j >= interval && v[j - interval] > temp; j -= interval) {
        v[j] = v[j - interval];
      }
      v[j] = temp;
    }
  }

    
  for (int i = 0; i < v.size(); i++)
    cout << v[i] << " ";
  cout << endl;
}
int main() {
    Lista<tupla> coordenadas;
    vector<float> v;
    int t;
    int h = 1;
    cout<<"Ingrese el tamaño de la lista: ";
    cin>>t;
    while(h<t+1){
        int x,y;
        cout<<"Ingrese x de la tupla "<<h<<": ";
        cin>>x;
        cout<<"Ingrese y de la tupla "<<h<<": ";
        cin>>y;
        tupla l=tupla(x,y);
        coordenadas.add(l);
        cout<<endl;
        h++;
    }
    int xx,yy;
    cout<<"¿Con cuál punto lo quiere comparar?"<<" Ingrese x: ";
    cin>>xx;
    cout<<"Ingrese y: ";
    cin>>yy;
    tupla x = tupla(xx,yy);
    v = coordenadas.distancias(x);
    shellSort(v);
    return 0;
}
