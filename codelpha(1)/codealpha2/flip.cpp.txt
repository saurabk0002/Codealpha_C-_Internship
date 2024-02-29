#include<iostream>
#include<fstream>
#include<string>
#include<filesystem>

using namespace std;
namespace fs = std::filesystem;

void create_directory(string path){
    if(fs::create_directory(path)){
        cout<<"Directory created successfully. \n";
    }else{
        cout<<"Failed to create directory. \n";
    }
}

void copy_file(string source, string destination){
    if(fs::copy_file(source, destination, fs::copy_options::overwrite_existing)){
        cout<<"File copied successfully. \n";
    }else{
        cout<<"Failed to copy file. \n";
    }
}

void move_file(const std::string& source, const std::string& destination){
    try{
        fs::rename(source, destination);
        std::cout<<"File moved successfully. \n";
    }catch(const std::filesystem::filesystem_error& e){
        std::cerr<<"Error moving file: :"<<e.what()<<std::endl;
    }
}

void list_directory(string path){
    for(const auto &entry : fs:: directory_iterator(path)){
        cout<<entry.path()<<endl;
    }
}

void read_file(string path){
    ifstream file(path);
    if(file.is_open()){
        string line;
        while(getline(file, line)){
            cout<<line<<endl;
        }
        file.close();
    }else{
        cout<<"Failed to open file. \n";
    }
}

void write_file(string path, string content){
    ofstream file(path);
    if(file.is_open()){
        file<<content;
        file.close();
        cout<<"File written successfully. \n";
    }else{
        cout<<"Failed to open file.";
    }
}

int main(){
    string command;
    string path;
    string source;
    string destination;
    string content;

    while(true){
        cout<<"Enter command (exit, cd, mkdir, cp, mv, ls, cat, echo): ";
        cin>>command;

        if(command == "exit"){
            break;
        }else if(command == "cd"){
            cin>>path;
            if(fs::exists(path)){
                fs::current_path(path);
            }else{
                cout<<"Directory does not exists. \n";
            }
        }else if(command == "mkdir"){
            cin>>path;
            create_directory(path);
        }else if(command == "cp"){
            cout<<"Enter Source";
            cin>>source;
            cout<<"Enter Destination";
            cin>>destination;
            copy_file(source, destination);
        }else if(command == "mv"){
            cout<<"Enter Source";
            cin>>source;
            cout<<"Enter Destination";
            cin>>destination;
            move_file(source, destination);
        }else if(command == "ls"){
            list_directory(fs::current_path());
        }else if(command == "cat"){
            cin>>path;
            read_file(path);
        }else if(command == "echo"){
            cin>>path;
            getline(cin, content);
            write_file(path, content);
        }else{
            cout<<"Invalid command. \n";
        }
    }
    return 0;
}