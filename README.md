error=0
def check_error(input_list):
    global error
    count=1
    count_1=0
    if ['hlt'] not in input_list:
        f2.write("Missing hlt instructions \n")
        error=1
    for i in input_list:
        if i[0]=='var':
            if var_error(input_list)==1 and count_1==0:
                f2.write("Variable not declared in the beginning \n")
                error=1
                count_1=1
           
        elif i[0]=='mov':
            if i[1] in register and (i[2]=="FLAGS" or i[2][0]=='$' or i[2] in register):
                continue
            else:
                f2.write("Illegal use of flag register \n")
                error=1
        elif i[0] not in opcode:
            error=1
            f2.write("Error in line no:"+str(i)+str(copy_list.index(i)+1)+"\n")
        else:
            newstmtyp=stmtypes[i[0]]
            if newstmtyp=='A':
                for j in i[1:]:
                    if j not in register:
                        error=1
                        f2.write("Invalid Register"+str(i)+str(copy_list.index(i)+1)+"\n")
                    else :
                        continue
            elif newstmtyp=='B':
                if i[1] not in register:
                    error=1
                    f2.write("Invalid register"+str(i)+str(copy_list.index(i)+1)+"\n")
                elif (i[2][1:].isdigit() and (int (i[2][1:])>127 or int(i[2][1:])<0)):
                    error=1
                    f2.write("Invalid Imm value"+str(i)+str(copy_list.index(i)+1)+"\n")
            elif newstmtyp=='C':
                for k in i[1:]:
                    if k not in register:
                        error=1
                        f2.write("Invalid Register"+str(i)+str(copy_list.index(i)+1)+"\n")       
                    else:
                        continue
            elif newstmtyp=="D":
                if i[1] not in register:
                    error=1
                    f2.write("Invalid register"+str(i)+str(copy_list.index(i)+1)+"\n")
                elif i[2] not in variables:
                    error=1
                    f2.write("Invalid varibale"+str(i)+str(copy_list.index(i)+1)+"\n")
            elif newstmtyp=="E":
                if i[1] not in labels: 
                    error=1
                    f2.write("label not found "+str(i)+str(copy_list.index(i)+1)+str('\n'))
                else:
                    continue
            elif newstmtyp=='F':
                if (copy_list.index(i)+1)<len(copy_list):
                    error=1
                    f2.write("hlt is not being used at last \n")
                else:
                    continue
            count+=1


check_error(input_list)
