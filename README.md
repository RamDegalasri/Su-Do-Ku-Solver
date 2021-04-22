# Su-Do-Ku-Solver
#include <iostream>
#include <cmath>
using namespace std;

bool canPlace(int mat[][9],int i,int j,int n,int number){
    for(int x=0;x<n;x++){
        //Row and Coln check
        if(mat[x][j] == number || mat[i][x] == number){
            return false;
        }
    }

    int rn = sqrt(n);
    int sx = (i/rn)*rn;
    int sy = (j/rn)*rn;

    for(int x=sx;x<sx+rn;x++){
        for(int y=sy;y<sy+rn;y++){
            if(mat[x][y] == number){
                return false;
            }
        }
    }
    return true;
}

bool SolveSudoku(int mat[][9],int i,int j,int n){
    //Base Case
    if(i == n){
        //Print matrix
        for(int i = 0;i < n;i++){
            for(int j = 0;j < n;j++){
                cout << mat[i][j] << " ";
            }
            cout << endl;
        }
        return true;
    }
    //Case Row end
    if(j == n){
        return SolveSudoku(mat,i+1,0,n);
    }
    //Skip the Pre-filled rows
    if(mat[i][j] != 0){
        return SolveSudoku(mat,i,j+1,n);
    }
    //Rec Case
    //Fill the current cell with possible options.
    for(int number=1;number<=9;number++){
        if(canPlace(mat,i,j,n,number)){
            mat[i][j] = number;
            
            bool couldWeSolve = SolveSudoku(mat,i,j+1,n);
            if(couldWeSolve == true){
                return true;
            }
        }
    }
    //Backtrack here
    mat[i][j] = 0;
    return false;
}

int main() {
    int mat[9][9];
    for(int  i = 0;i < 9;i++){
        for(int j = 0;j < 9;j++){
            cin >> mat[i][j];
        }
    }
    SolveSudoku(mat,0,0,9);
}
