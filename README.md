数据库辅助类
通过反射实现数据库的增删改查
在数据库辅助类中实现了自动建表,自动添加表字段
@Table("Person")
public class Person {
    @Column(value="id",id=true)
    private int id;
    @Column("name")
    private String name;
    @Column("age")
    private int age;
    @Column("birthday")
    private Date birthday;
    @Column("sex")
    private boolean sex;
}
表名和字段名是可以自己确定的,会根据变量的类型的自动给予数据库字段类型,主键自增.

建议在application中初始化db,或者contentprovider.
Db.getInstance().init(new TableHelper(context,"db",null,1));

初始化后,就可以使用Db.getInstance()来进行数据库操作
封装了一些常用的数据库操作,单对象的插入,数组的插入,通过id更新等等,还有通过sql语句查询数组
Db.getInstance().save(objcect);
Object o =Db.getInstance().queryById(class,id);

在TableHelper中,实现对应实体类包路径下的实体进行自动建表
    @Override
    public void onCreate(SQLiteDatabase db) {
        try{
            List<String> lstClassNames=getClassName(packageName);
            for (String className:
                    lstClassNames) {
                Class clazz=Class.forName(className);
                buildTable(clazz,db);
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
