# Skylark-Drones---ALANKAR-VERMA
Solution to Problem Statement: Write a program that solves word-chain puzzles.
#include <bits/stdc++.h>

using namespace std;
string startword,endword,*words;
int startwordl,endwordl,nd,wordl;
int graph[10000][10000];


void bfs()
{
    queue<int> q;
    bool visited[nd];
    int dist[nd];
    for(int i=0;i<nd;i++)                    //distance initialized
    {
        dist[i]=INT_MAX;
    }
    dist[0]=0;
    for(int i=0;i<nd;i++)
    {
        visited[i]=false;
    }
    q.push(0);
    while(!q.empty())
    {
        int node=q.front();                 //taking out first element og the queue
        q.pop();
        if(visited[node]==false)
        {
        visited[node]=true;
        int newdist;
        for(int i=0;i<nd;i++)
        {
            if(graph[node][i]==1 && visited[i]==false)    //checking for connected elements from the popped element
            {
                newdist=dist[node]+1;                      //modify the distance of the connected
                if(newdist<dist[i])
                {
                    dist[i]=newdist;
                }
                q.push(i);                                //pushing the connected element
            }
        }
        }
    }



    if(dist[nd-1]==INT_MAX)                         //checking if end node is not reachable
    {
        cout<<"not possible\n";
        return;
    }

        cout<<"no of steps:"<<dist[nd-1]+1<<"\n";       //required no of steps

       cout<<"path:\n";

    //GENERATING THE PATH
    int searchindex=nd-1;                     //two variables used- searchindex used for element form which the reachable nodes determined
    int curdist=dist[nd-1]-1;                 //curdist used for the comparision distance of reachable nodes
    list<int> li;
    li.push_front(nd-1);
    while(curdist!=0)
    {
        for(int i=0;i<nd-1;i++)
        {
            if(graph[searchindex][i]==1 && dist[i]==curdist)  //indexwise first reachable node with dist[i]=curdist taken
            {
                li.push_front(i);
                searchindex=i;
                curdist--;
                break;
            }
        }
    }
    li.push_front(0);
    for(list<int>::iterator it=li.begin();it!=li.end();it++)
    {
        int x=*it;
        cout<<words[x]<<"\n";                                //printing the required words
    }
}




int main()
{
    cout<<"\nEnter the starting and ending words:";
    cin>>startword>>endword;
    startwordl=startword.length();       //startwordl=length of startword
    endwordl=endword.length();           //endwordl=length of endword
    if(startwordl!=endwordl)
    {
        cout<<"\nNot possible";
        return 0;
    }
    wordl=startwordl;
    cout<<"\nEnter the no of elements in the dictionary other than the starting and ending words:";
    cin>>nd;
    words=new string[nd+2];
    words[0]=startword;        //first element of words array is set to startword

    int index=1;
    int i=0;
    string w;
    if(nd!=0)
    cout<<"\nEnter the words:";
    while(i<nd)
    {
        cin>>w;
        if(w.length()==wordl)    //checking for valid length if valid taken into words array
        {
            words[index]=w;
            index++;             //index increases only for valid words
        }
        i++;
    }
    words[index]=endword;
    nd=index+1;
    for(int i=0;i<nd;i++)
    {
        for(int j=0;j<nd;j++)
        {
            int cnt=0;
            for(int k=0;k<wordl;k++)     //checking for words having only one change in character
            {
                if(words[i][k]!=words[j][k])
                {
                    cnt++;
                }
            }
            if(cnt==1)                  //make the node connected in the graph if change is 1
            {
                graph[i][j]=1;
            }

            else
            {
                graph[i][j]=-1;
            }
        }
    }


    bfs();                               //call breadth first search

    return 0;
}
