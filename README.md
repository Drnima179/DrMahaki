# DrMahaki
import { useState } from "react"; import { Button } from "@/components/ui/button"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Calendar } from "@/components/ui/calendar"; import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs"; import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";

export default function ClinicWebsite() { const [name, setName] = useState(""); const [phone, setPhone] = useState(""); const [date, setDate] = useState<Date | undefined>(); const [submitted, setSubmitted] = useState(false); const [appointments, setAppointments] = useState<{ name: string; phone: string; date: Date }[]>([]); const [editIndex, setEditIndex] = useState<number | null>(null);

const handleSubmit = () => { if (name && phone && date) { const newAppointment = { name, phone, date }; if (editIndex !== null) { const updatedAppointments = [...appointments]; updatedAppointments[editIndex] = newAppointment; setAppointments(updatedAppointments); setEditIndex(null); } else { setAppointments([...appointments, newAppointment]); } setSubmitted(true); setName(""); setPhone(""); setDate(undefined); setTimeout(() => setSubmitted(false), 3000); } };

const handleEdit = (index: number) => { const appt = appointments[index]; setName(appt.name); setPhone(appt.phone); setDate(appt.date); setEditIndex(index); };

const handleDelete = (index: number) => { const filtered = appointments.filter((_, i) => i !== index); setAppointments(filtered); };

return ( <div className="min-h-screen bg-pink-50 p-6"> <h1 className="text-4xl font-bold text-center text-pink-800 mb-8"> کلینیک زیبایی دکتر نیما محکی </h1>

<Tabs defaultValue="booking" className="max-w-4xl mx-auto">
    <TabsList className="grid w-full grid-cols-5 mb-6">
      <TabsTrigger value="booking">رزرو نوبت</TabsTrigger>
      <TabsTrigger value="about">درباره ما</TabsTrigger>
      <TabsTrigger value="services">خدمات</TabsTrigger>
      <TabsTrigger value="gallery">گالری</TabsTrigger>
      <TabsTrigger value="dashboard">پنل مدیریت</TabsTrigger>
    </TabsList>

    <TabsContent value="booking">
      <Card className="shadow-xl">
        <CardContent className="p-6 space-y-4">
          <h2 className="text-2xl font-semibold text-pink-700">رزرو نوبت آنلاین</h2>
          {submitted && <p className="text-green-600">نوبت شما با موفقیت ثبت شد.</p>}
          <Input
            placeholder="نام و نام خانوادگی"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <Input
            placeholder="شماره تماس"
            value={phone}
            onChange={(e) => setPhone(e.target.value)}
          />
          <Calendar
            mode="single"
            selected={date}
            onSelect={setDate}
            className="rounded-md border"
          />
          <Button className="w-full bg-pink-600 text-white" onClick={handleSubmit}>
            {editIndex !== null ? "ویرایش نوبت" : "ثبت نوبت"}
          </Button>
        </CardContent>
      </Card>
    </TabsContent>

    <TabsContent value="about">
      <Card className="shadow-md">
        <CardContent className="p-6 text-justify leading-7">
          کلینیک زیبایی دکتر نیما محکی با بهره‌گیری از تجهیزات روز دنیا و تیمی متخصص، خدمات متنوعی در زمینه زیبایی، جوان‌سازی پوست، و مراقبت‌های پیشرفته ارائه می‌دهد.
        </CardContent>
      </Card>
    </TabsContent>

    <TabsContent value="services">
      <Card className="shadow-md">
        <CardContent className="p-6 space-y-2">
          <ul className="list-disc px-5 space-y-1 text-right">
            <li>لیزر موهای زائد</li>
            <li>تزریق ژل و بوتاکس</li>
            <li>هایفوتراپی</li>
            <li>میکرونیدلینگ و مزوتراپی</li>
            <li>پاکسازی و فیشیال صورت</li>
          </ul>
        </CardContent>
      </Card>
    </TabsContent>

    <TabsContent value="gallery">
      <Card className="shadow-md">
        <CardContent className="p-6 grid grid-cols-2 md:grid-cols-3 gap-4">
          <div className="h-32 bg-pink-100 rounded-lg" />
          <div className="h-32 bg-pink-200 rounded-lg" />
          <div className="h-32 bg-pink-300 rounded-lg" />
          {/* تصاویر واقعی بعداً اضافه شود */}
        </CardContent>
      </Card>
    </TabsContent>

    <TabsContent value="dashboard">
      <Card className="shadow-md">
        <CardContent className="p-6">
          <h2 className="text-2xl font-semibold mb-4">لیست نوبت‌های ثبت‌شده</h2>
          <Table>
            <TableHeader>
              <TableRow>
                <TableHead>نام بیمار</TableHead>
                <TableHead>شماره تماس</TableHead>
                <TableHead>تاریخ نوبت</TableHead>
                <TableHead>عملیات</TableHead>
              </TableRow>
            </TableHeader>
            <TableBody>
              {appointments.map((appt, index) => (
                <TableRow key={index}>
                  <TableCell>{appt.name}</TableCell>
                  <TableCell>{appt.phone}</TableCell>
                  <TableCell>{appt.date.toLocaleDateString()}</TableCell>
                  <TableCell className="space-x-2">
                    <Button size="sm" onClick={() => handleEdit(index)}>ویرایش</Button>
                    <Button size="sm" variant="destructive" onClick={() => handleDelete(index)}>حذف</Button>
                  </TableCell>
                </TableRow>
              ))}
            </TableBody>
          </Table>
          {appointments.length === 0 && <p className="text-gray-500 mt-4">هیچ نوبتی ثبت نشده است.</p>}
        </CardContent>
      </Card>
    </TabsContent>
  </Tabs>
</div>

); }

