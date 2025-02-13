### `Many to Many`
```sql
create table StCou(
scid int primary key auto_increment,
fsid int,
fcid int,
Foreign key(fsid) references student(sid) on delete cascade on update cascade,
Foreign key(fcid) references course(cid) on delete cascade on update cascade
);
```

### Student_Details
```sql
create table Studentdetails(
Sdid int primary key auto_increment,
Mnum Varchar(35),
fsid int,
Foreign key(fsid) references student(sid) on delete cascade on update cascade
);
```

### Course Table
```sql
create table Course(
cid int primary key auto_increment,
cname varchar(35) Not null,
cfee Double
);
```

### Student
```sql
create table student(
sid int primary key auto_increment,
sname varchar(35) Not null,
sage int
);
```
