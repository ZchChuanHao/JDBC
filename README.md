# JDBC
package Test;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class StuDao {
	
	// 添加增加方法；
//	public int addStu(Tstudent tstudent){
//		Connection conn = DBUtil.getConnection();
//		int i = 0;
//		//创建statement
//		Statement stmt;
//		try {
//			//执行sql
//			String sql = "insert into t_student values("+tstudent.getSid()+"','"+tstudent.getName()+"','"+tstudent.getSex()+"','"+tstudent.getBirth()+"','"+tstudent.getStel()+"',"+tstudent.getClass()+")";
//
//			stmt = conn.createStatement();
//			i = stmt.executeUpdate(sql)	;
//			stmt.close();
//			conn.close();
//		} catch (SQLException e) {
//			// TODO Auto-generated catch block
//			e.printStackTrace();
//		}
//		
//		return i;
//	}
	
	public int addStu(Tstudent tstudent){
		Connection conn = DBUtil.getConnection();
		int i = 0;
		//创建statement
		PreparedStatement ps;
		try {
			//执行sql
			String sql = "insert into t_student values(sid=?,sname=?,ssex=?,sbirthday=?,stel=?,sclass=?)";
			ps = conn.prepareStatement(sql);
			ps.setInt(1, tstudent.getSid());
			ps.setInt(1, tstudent.getSid());
			ps.setString(2, tstudent.getName());
			ps.setString(3, tstudent.getSex());
			ps.setString(4, tstudent.getBirth());
			ps.setString(5, tstudent.getStel());
			ps.setInt(6,tstudent.getSclass());
			i= ps.executeUpdate();
			ps.close();
			conn.close();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		return i;
	}
	
	
	
	
	// 删除功能
	public int delete(int sid){
		Connection conn = DBUtil.getConnection();
		String sql ="delete  from t_student where cid = ?";   //?:占位符
		PreparedStatement ps;
		int i=0;
		try {
			ps = conn.prepareStatement(sql);
			ps.setInt(1, sid);
			i = ps.executeUpdate();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}return i;
		
	}
	// 更新功能
	public int update(Tstudent tstudent){
		Connection conn = DBUtil.getConnection();
		String sql = "update t_studnet SET sid=?,sname=?,ssex=?,sbirthday=?,stel=?,sclass=?";
		PreparedStatement ps = null;
		int i = 0;
		try {
			ps = conn.prepareStatement(sql);
			ps.setInt(1, tstudent.getSid());
			ps.setString(2, tstudent.getName());
			ps.setString(3, tstudent.getSex());
			ps.setString(4, tstudent.getBirth());
			ps.setString(5, tstudent.getStel());
			ps.setInt(6,tstudent.getSclass());
			i = ps.executeUpdate();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return i;
	}
	
	//查询所有功能
	public List<Tstudent> getStudentList(){
		List<Tstudent> stulist = new ArrayList<Tstudent>();
		Connection conn = DBUtil.getConnection();
		Statement stmt;
		try {
			stmt = conn.createStatement();
			String sql = "select * from t_student";
			ResultSet rs = stmt.executeQuery(sql);
			while(rs.next()){
				int sid = rs. getInt(1);
				//将查询的改行的每一个字段存入一个新的Tstudent集合中
				String name = rs.getString(2);
				String sex = rs.getString(3);
				String birth = rs.getString(4);
				String stel = rs.getString(5);
				int classid = rs.getInt(6);
				Tstudent tst = new Tstudent(sid,name,sex,birth,stel,classid);
				stulist.add(tst);
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		return stulist;
	}
//	public Tstudent getStudentSid(int sid){
//		return null;
//	}
}




-----------------------------------------------------------------------------------------
package Test;

import java.util.List;
import java.util.Scanner;

public class Student {
	static Scanner sc = new Scanner(System.in);
	static StuDao dao = new StuDao();
	public static void main(String[] args){
		while(true){
		System.out.println("==============welcome==============");
		System.out.println("1，增加 2.修改 3.删除 4.查询");
		System.out.println("请选择要执行的操作");
		System.out.println("--------------分界线-----------------");
		int i = sc.nextInt();
		switch(i){
		case 1:
			add();
			break;
		case 2:
			update();
			break;
		case 3:
			delete();
			break;
		case 4:
			getlist();
			break;
		default :
			System.out.println("error!!!!");
			break;
		}
	}
	}
	private static void add(){
		System.out.println("请输入学号");
		int sid = sc.nextInt();
		System.out.println("请输入学生姓名");
		String sname = sc.next();
		System.out.println("请输入学生性别");
		String sex = sc.next();
		System.out.println("请输入出生年月");
		String birth = sc.next();
		System.out.println("请输入电话号码");
		String stel = sc.next();
		System.out.println("请输入班级号");
		int sclass = sc.nextInt();
//		Tstudent tstu = new Tstudent(sid,sname);
		Tstudent tstu = new Tstudent(sid,sname,sex,birth,stel,sclass);
		//调用dao中的方法。将输入的信息传入。
		int i = dao.addStu(tstu);
		if(i>0){
			System.out.println("增加成功");
		}else{
			System.out.println("增加失败");
		}
	}
	private static void getlist(){
		List<Tstudent> stulist = dao.getStudentList();
		for(int i =0;i<stulist.size();i++){
			System.out.println(i);
		}
	}
	private static void delete(){
		System.out.println("输入sid");
		int id = sc.nextInt();
		int i = dao.delete(id);
		if(i>0){
			System.out.println("删除成功");
		}else{
			System.out.println("删除失败");
		}
	}
	private static void update(){
		System.out.println("请输入要修改的学号");
		int a =sc.nextInt();

		
	}
}
