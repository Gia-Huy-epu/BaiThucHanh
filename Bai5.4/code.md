using System;
using System.Collections.Generic;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using System.Windows.Forms;
namespace WinFormsApp4
{
    public partial class Form1 : Form
    {
        public class Employee
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Position { get; set; }
            public string JoinDate { get; set; }
            public string GroupKey { get; set; }

            public Employee(string id, string name, string position, string joinDate, string groupKey)
            {
                Id = id;
                Name = name;
                Position = position;
                JoinDate = joinDate;
                GroupKey = groupKey;
            }
        }

        private List<Employee> employeeList = new List<Employee>();
        public Form1()
        {
            InitializeComponent();
        }
        private void Form5_4_Load(object sender, EventArgs e)
        {
            cboViewMode.Items.AddRange(Enum.GetNames(typeof(View)));
            cboViewMode.SelectedItem = View.Details.ToString();

            TreeNode root = new TreeNode("Công ty ABC") { Tag = "ROOT" };

            TreeNode nodeIT = new TreeNode("Phòng IT") { Tag = "IT" };
            TreeNode nodeIT1 = new TreeNode("Nhóm Phầm mềm") { Tag = "IT_SW" };
            TreeNode nodeIT2 = new TreeNode("Nhóm Mạng") { Tag = "IT_NET" };
            nodeIT.Nodes.Add(nodeIT1);
            nodeIT.Nodes.Add(nodeIT2);

            TreeNode nodeHR = new TreeNode("Phòng Nhân sự") { Tag = "HR" };
            TreeNode nodeHR1 = new TreeNode("Nhóm Tuyển dụng") { Tag = "HR_REC" };
            nodeHR.Nodes.Add(nodeHR1);

            root.Nodes.Add(nodeIT);
            root.Nodes.Add(nodeHR);

            tvDepartments.Nodes.Add(root);
            tvDepartments.ExpandAll();

            employeeList.Add(new Employee("NV01", "Nguyễn Văn A", "Trưởng nhóm", "10/01/2020", "IT_SW"));
            employeeList.Add(new Employee("NV02", "Trần Thị B", "Lập trình viên", "15/03/2021", "IT_SW"));
            employeeList.Add(new Employee("NV03", "Lê Văn C", "Kỹ sư Mạng", "01/06/2019", "IT_NET"));
            employeeList.Add(new Employee("NV04", "Phạm Thị D", "Chuyên viên TD", "20/08/2022", "HR_REC"));
        }

        private void tvDepartments_AfterSelect(object sender, TreeViewEventArgs e)
        {
            lsvEmployees.Items.Clear();
            string selectedTag = e.Node.Tag?.ToString();

            if (string.IsNullOrEmpty(selectedTag)) return;

            foreach (var emp in employeeList)
            {
                if (selectedTag == "ROOT" || emp.GroupKey == selectedTag || emp.GroupKey.StartsWith(selectedTag))
                {
                    ListViewItem item = new ListViewItem(emp.Id);
                    item.SubItems.Add(emp.Name);
                    item.SubItems.Add(emp.Position);
                    item.SubItems.Add(emp.JoinDate);

                    lsvEmployees.Items.Add(item);
                }
            }
        }

        private void cboViewMode_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (Enum.TryParse(cboViewMode.SelectedItem.ToString(), out View selectedView))
            {
                lsvEmployees.View = selectedView;
            }
        }
    }
}
