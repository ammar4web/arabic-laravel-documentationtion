### المقدمة

عندما تقوم باختبار تطبيقك أو تعبئة قاعدة البيانات ببيانات تجريبية (seeding)، قد تحتاج إلى إدخال بعض البيانات (السجلات) في قاعدة البيانات.
بدلاً من كتابة قيمة كل عمود يدويًا، يسمح لك Laravel بتحديد مجموعة من القيم الافتراضية (الخصائص) لكل نموذج Eloquent عن طريق **factories** (المصانع).
لكي ترى مثالًا على كيفية كتابة factory، يمكنك النظر داخل تطبيقك في الملف: `database/factories/UserFactory.php`   
هذا الملف يأتي مع جميع تطبيقات Laravel الجديدة، ويحتوي على تعريف factory مثل المثال التالي:

```php
namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;
use Illuminate\Support\Facades\Hash;
use Illuminate\Support\Str;

/**
 * @extends \Illuminate\Database\Eloquent\Factories\Factory<\App\Models\User>
 */
class UserFactory extends Factory
{
    /**
     * The current password being used by the factory.
     */
    protected static ?string $password;

    /**
     * Define the model's default state.
     *
     * @return array<string, mixed>
     */
    public function definition(): array
    {
        return [
            'name' => fake()->name(),
            'email' => fake()->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => static::$password ??= Hash::make('password'),
            'remember_token' => Str::random(10),
        ];
    }

    /**
     * Indicate that the model's email address should be unverified.
     */
    public function unverified(): static
    {
        return $this->state(fn (array $attributes) => [
            'email_verified_at' => null,
        ]);
    }
}
```

كما ترى، في أبسط صورة، **factories** (المصانع) هي عبارة عن **كلاسات (classes)** ترث من الكلاس الأساسي للمصانع (**factories**) في Laravel، وتحتوي على **دالة اسمها `definition`**.

هذه الدالة ترجع مجموعة من القيم الافتراضية للخصائص (attributes) التي سيتم استخدامها عند إنشاء نموذج (model) باستخدام المصنع.

من خلال دالة **`fake`**، يمكن للمصانع استخدام مكتبة **Faker** الخاصة بلغة PHP، والتي تتيح لك توليد بيانات عشوائية بسهولة لاستخدامها في الاختبارات وتعبئة البيانات.

---

