# cl_pr_1

namespace Applications.School
{
public class School
{
public List<Course> courses { get; set; }
public List<Student> students { get; set; }

        public List<Teacher> teachers { get; set; }
        School()
        {

        }

        public Teacher GetTeacherByCourseId(string courseId)
        {
            if (courseId != null && courses.Count() > 0 && courses.Select(c=>c.courseId).Contains(courseId))
            {
                return (Teacher)courses.Where(c => c.courseId == courseId).Select(c => c.teacher);
            }
            return null;
        }

        public string GetGradeByCourseIdAndStudent(string courseId, int studentId)
        {
            if (courseId != null && students.Count() > 0 && students.Select(c => c.studentID).Contains(studentId))
            {
                Student student = (Student)students.Where(c => c.studentID == studentId);
                List<Tuple<Course, string>> courseGrade = student.courseGrades.Where(c => c.Item1.courseId == courseId).ToList();
                if (courseGrade.Count > 0)
                {
                    return courseGrade[0].Item2;
                }
            }
            return null;
        }


    }

    public class Course
    {
        public string courseId { get; set; }
        public int year { get; set; }
        public string semester { get; set; }
        public Teacher teacher { get; set; }

        Course()
        {
        }

    }

    public class Teacher
    {
        public int teacherId { get; set; }
        Teacher()
        {

        }
    }

    public class Student
    {
        public int studentID { get; set; }
        public decimal averageGrade { get; set; }
        public List<Course> courseIds { get; set; }
        public List<Tuple<Course, string>> courseGrades {get; set;}
        Student()
        {

        }

        public decimal GetAverage(int year, string semester)
        {
            double total = 0;
            if (courseGrades.Count > 0)
            {
                List<Tuple<Course, string>> courseGradesSelected = courseGrades.Where(cg => cg.Item1.year == year && cg.Item1.semester == semester).ToList();
                foreach(var cg in courseGradesSelected)
                {
                    if (cg.Item2.StartsWith("A"))
                    {
                        total += 4;
                        if (!cg.Item2.Contains("+"))
                        {
                            total -= .2;
                        }
                        else if (cg.Item2.Contains("-"))
                        {
                            total -= .4;
                        }
                    }
                    else if (cg.Item2.StartsWith("B"))
                    {
                        total += 3;
                        if (!cg.Item2.Contains("+"))
                        {
                            total -= .2;
                        }
                        else if (cg.Item2.Contains("-"))
                        {
                            total -= .4;
                        }
                    }
                    else if (cg.Item2.StartsWith("C"))
                    {
                        total += 2;
                        if (!cg.Item2.Contains("+"))
                        {
                            total -= .2;
                        }
                        else if (cg.Item2.Contains("-"))
                        {
                            total -= .4;
                        }
                    }
                    else if (cg.Item2.StartsWith("D"))
                    {
                        total += 1;
                        if (!cg.Item2.Contains("+"))
                        {
                            total -= .2;
                        }
                        else if (cg.Item2.Contains("-"))
                        {
                            total -= .4;
                        }
                    }
                    if (cg.Item2.StartsWith("F"))
                    {
                        total += 0;
                    }
                }
                return (decimal)(total / courseGradesSelected.Count);
            }
            return 0;
        }
    }

}
