## Một số hàm C#
- Đổi trường của currentContext:TandanCore2 > Tandan.Assets > Controls > ClientContext
- Convert Object to Json
```csharp
public string ConvertObjectToJSON(object obj)
        {
            JavaScriptSerializer result = new JavaScriptSerializer();
            return result.Serialize(obj);
        }
new JavaScriptSerializer().Serialize(data); 
```
- Check user in group
```csharp
public static string CheckUserInGroups(string _groups)
    {
        string result = "";
        try
        {
            SPUser curUser = SPContext.Current.Web.CurrentUser;
            string[] groups = _groups.Split(new string[] { "##" }, StringSplitOptions.RemoveEmptyEntries);
            List<string> _result = new List<string>();
            foreach (string group in groups)
            {
                if (TDPermission.IsUserMemberOfGroup(curUser, group) == true)
                    _result.Add("1");
                else
                    _result.Add("0");
            }
            result = string.Join("#", _result.ToArray());
        }
        catch (Exception ex) { WriteLogs("CheckUserInGroups", ex + ""); }
        return result;
    }
```
- Đọc multilinetext có thẻ div
```csharp
ykien = "<data>" + obj.YKien + "</data>";
XmlDocument xml = new XmlDocument();
xml.LoadXml(ykien);
ykien = xml.SelectSingleNode("data").InnerText;
```
- Vượt quyền
```csharp
SPSecurity.RunWithElevatedPrivileges(delegate() {
	using(SPSite oSite = new SPSite(siteUrl)) {
		using(SPWeb oWeb = oSite.OpenWeb("/qlcc")) {}
	}
});
```
- Lấy tuần từ ngày
```csharp
public static int GetIso8601WeekOfYear(DateTime time)
{
    DayOfWeek day = CultureInfo.InvariantCulture.Calendar.GetDayOfWeek(time);
    if (day >= DayOfWeek.Monday && day <= DayOfWeek.Wednesday)
    {
    	time = time.AddDays(3);
    }
	return CultureInfo.InvariantCulture.Calendar.GetWeekOfYear(time, CalendarWeekRule.FirstFourDayWeek, DayOfWeek.Monday);
} 
```
- Split
```csharp
str.Split( new char[] {','}, StringSplitOptions.RemoveEmptyEntries)
```
- chuyển thành số dạng roman
```csharp
private string ToRoman(int number)
{
    if ((number < 0) || (number > 4999)) throw new ArgumentOutOfRangeException("insert value betwheen 1 and 3999");
    if (number < 1) return string.Empty;
    if (number >= 1000) return "M" + ToRoman(number - 1000);
    if (number >= 900) return "CM" + ToRoman(number - 900);
    if (number >= 500) return "D" + ToRoman(number - 500);
    if (number >= 400) return "CD" + ToRoman(number - 400);
    if (number >= 100) return "C" + ToRoman(number - 100);
    if (number >= 90) return "XC" + ToRoman(number - 90);
    if (number >= 50) return "L" + ToRoman(number - 50);
    if (number >= 40) return "XL" + ToRoman(number - 40);
    if (number >= 10) return "X" + ToRoman(number - 10);
    if (number >= 9) return "IX" + ToRoman(number - 9);
    if (number >= 5) return "V" + ToRoman(number - 5);
    if (number >= 4) return "IV" + ToRoman(number - 4);
    if (number >= 1) return "I" + ToRoman(number - 1);
    throw new ArgumentOutOfRangeException("something bad happened");
}
```
- thay thế ký tự đặc biệt bằng CHỮ và SỐ
```csharp
str = Regex.Replace(str, "[^\\w\\d]", "");
```
- Đọc số thành chữ
```csharp
public String NumberToTextVN(decimal total)
    {
        try
        {
            string rs = "";
            total = Math.Round(total, 0);
            string[] ch = { "không", "một", "hai", "ba", "bốn", "năm", "sáu", "bảy", "tám", "chín" };
            string[] rch = { "lẻ", "mốt", "", "", "", "lăm" };
            string[] u = { "", "mươi", "trăm", "nghìn", "", "", "triệu", "", "", "tỷ", "", "", "ngàn", "", "", "triệu" };
            string nstr = total.ToString();

            int[] n = new int[nstr.Length];
            int len = n.Length;
            for (int i = 0; i < len; i++)
            {
                n[len - 1 - i] = Convert.ToInt32(nstr.Substring(i, 1));
            }

            for (int i = len - 1; i >= 0; i--)
            {
                if (i % 3 == 2)// số 0 ở hàng trăm
                {
                    if (n[i] == 0 && n[i - 1] == 0 && n[i - 2] == 0) continue;//nếu cả 3 số là 0 thì bỏ qua không đọc
                }
                else if (i % 3 == 1) // số ở hàng chục
                {
                    if (n[i] == 0)
                    {
                        if (n[i - 1] == 0) { continue; }// nếu hàng chục và hàng đơn vị đều là 0 thì bỏ qua.
                        else
                        {
                            rs += " " + rch[n[i]]; continue;// hàng chục là 0 thì bỏ qua, đọc số hàng đơn vị
                        }
                    }
                    if (n[i] == 1)//nếu số hàng chục là 1 thì đọc là mười
                    {
                        rs += " mười"; continue;
                    }
                }
                else if (i != len - 1)// số ở hàng đơn vị (không phải là số đầu tiên)
                {
                    if (n[i] == 0)// số hàng đơn vị là 0 thì chỉ đọc đơn vị
                    {
                        if (i + 2 <= len - 1 && n[i + 2] == 0 && n[i + 1] == 0) continue;
                        rs += " " + (i % 3 == 0 ? u[i] : u[i % 3]);
                        continue;
                    }
                    if (n[i] == 1)// nếu là 1 thì tùy vào số hàng chục mà đọc: 0,1: một / còn lại: mốt
                    {
                        rs += " " + ((n[i + 1] == 1 || n[i + 1] == 0) ? ch[n[i]] : rch[n[i]]);
                        rs += " " + (i % 3 == 0 ? u[i] : u[i % 3]);
                        continue;
                    }
                    if (n[i] == 5) // cách đọc số 5
                    {
                        if (n[i + 1] != 0) //nếu số hàng chục khác 0 thì đọc số 5 là lăm
                        {
                            rs += " " + rch[n[i]];// đọc số 
                            rs += " " + (i % 3 == 0 ? u[i] : u[i % 3]);// đọc đơn vị
                            continue;
                        }
                    }
                }

                rs += (rs == "" ? " " : ", ") + ch[n[i]];// đọc số
                rs += " " + (i % 3 == 0 ? u[i] : u[i % 3]);// đọc đơn vị
            }
            //if (rs[rs.Length - 1] != ' ')
            //    rs += " đồng";
            //else
            //    rs += "đồng";

            if (rs.Length > 2)
            {
                string rs1 = rs.Substring(0, 2);
                rs1 = rs1.ToUpper();
                rs = rs.Substring(2);
                rs = rs1 + rs;
            }
            return rs.Trim().Replace(",", "");
        }
        catch
        {
            return "";
        }

    }
```
- Load HTML từ text
```csharp
// From File
var doc = new HtmlDocument();
doc.Load(filePath);

// From String
var doc = new HtmlDocument();
doc.LoadHtml(html);

// From Web
var url = "http://html-agility-pack.net/";
var web = new HtmlWeb();
var doc = web.Load(url);
```
- join in syntax
```csharp
 var _node = from p in items1 join q in items2 on p.SoKy equals q.Ma select new { p, q.ThuTu };
 var items = from x in _node orderby x.p.NamBaoCaoText, x.ThuTu select x.p;
```
- Get public tokenkey
	- Get Public Token Key
	- C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\Bin\x64\sn.exe
	- -Tp $(TargetPath)
	- Check User Output Windows

- Xem file trực tiếp
```csharp
if (!file.Contains("http"))
    file = oSite.Url + file;

if (dinhDang == "mp4" || dinhDang == "avi" || dinhDang == "mov" || dinhDang == "mpeg" || dinhDang == "wmv")
    str += CreatePortlet(ten, "<video width='100%' height='auto' controls><source src='" + file + "' type='video/mp4'></video>", "6", j);
else if (dinhDang == "jpg" || dinhDang == "jpeg" || dinhDang == "png")
    str += CreatePortlet(ten, "<img width='100%' height='auto' src='" + file + "' alt='" + ten + "'/>", "6", j);
else if (dinhDang == "doc" || dinhDang == "docx" || dinhDang == "txt" || dinhDang == "xls" || dinhDang == "xlsx" || dinhDang == "ppt" || dinhDang == "pptx")
    str += CreatePortlet(ten, "<iframe src='https://view.officeapps.live.com/op/embed.aspx?src=" + file + "' width='100%' height='400px;' frameborder='0'>This is an embedded </iframe>", "12", j);
else if (dinhDang == "pdf") {
    str += CreatePortlet(ten, "<embed src='https://drive.google.com/viewerng/viewer?embedded=true&url=" + file + "' width='100%' height='400px;'>", "12", j);
} else
    str += "<div class='col-lg-6 col-xs-6 col-md-6 fileDinhKem fileDinhKem" + j + "'><a href='" + file + "' targer='_blank'><span><img src='" + oSite.Url + "/portal/AlbumPhoto/DinhDangFile/" + dinhDang.ToLower() + ".png'/></span><span>" + ten + "</span></a></div>";


public string CreatePortlet(string name, string content, string size, int j)
{
	return "<div class='col-lg-" + size + " col-xs-" + size + " col-md-" + size + "fileDinhKem fileDinhKem" + j + "'><div class=\"portlet light bordered\"><div class=\"portlet-title\" style=\"padding:10px;\"><div class=\"caption\"><span id=\"PlaceHolderName\">" + name + "</span></div></div><div class=\"portlet-body\">" + content + "</div></div></div>";
}
```