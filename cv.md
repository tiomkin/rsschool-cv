# Artsiom Burdukou

## Contacts
* Mobile Telephone, Viber, Telegram: +370 609-763-23
* E-mail: artsiomburdukou@gmail.com
* [Facebook: https://www.facebook.com/tim.burdukov/](https://www.facebook.com/tim.burdukov/)
* [LinkedIn: https://www.linkedin.com/in/artsiom-burdukou-94472849/](https://www.linkedin.com/in/artsiom-burdukou-94472849/)

## Brief Information
I am a lawyer who decided to change his life because of relocating to Vilnius.\
I dream to turn my previous web-programming hobby into the main job.\
As a lawyer I know what does it mean *"to work hard"*, *"researching"* and *"keep learning"*.\
I think my best features are:
* perseverance
* responsibility
* good nature

## Skills
C#, .NET Core, ASP.NET Core, HTML, CSS, Git, GitHub, GitLab, Visual Studio, Visual Studio Code

## Code Example
AdminController class (managing web-site users) from my education ASP.NET Core MVC project:
```
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Mvc;
using ProjectToRemember.Models;
using ProjectToRemember.Models.ViewModels;
using System;
using System.Threading.Tasks;

namespace ProjectToRemember.Controllers
{
    public class AdminController : Controller
    {
        private UserManager<AppUser> userManager;
        private IUserValidator<AppUser> userValidator;
        private IPasswordValidator<AppUser> passwordValidator;
        private IPasswordHasher<AppUser> passwordHasher;
        public AdminController(UserManager<AppUser> mgr,
            IUserValidator<AppUser> uVal,
            IPasswordValidator<AppUser> pVal,
            IPasswordHasher<AppUser> pHash)
        {
            userManager = mgr;
            userValidator = uVal;
            passwordValidator = pVal;
            passwordHasher = pHash;
        }

        public IActionResult Index() => View(userManager.Users);

        public IActionResult Create() => View();

        [HttpPost]
        public async Task<IActionResult> Create(CreateModel model)
        {
            if (ModelState.IsValid)
            {
                AppUser user = new()
                {
                    UserName = model.Name,
                    Email = model.Email
                };

                IdentityResult result = await userManager.CreateAsync(user, model.Password);

                if (result.Succeeded)
                {
                    return RedirectToAction("Index");
                }
                else
                {
                    foreach (var error in result.Errors)
                    {
                        ModelState.AddModelError("", error.Description);
                    }
                }
            }

            return View(model);
        }

        [HttpPost]
        public async Task<IActionResult> Delete(string id)
        {
            AppUser user = await userManager.FindByIdAsync(id);

            if (user != null)
            {
                IdentityResult result = await userManager.DeleteAsync(user);

                if (result.Succeeded)
                {
                    return RedirectToAction("Index");
                }
                else
                {
                    AddErrorsFromResult(result);
                }
            }
            else
            {
                ModelState.AddModelError("", "Пользователь не найден");
            }

            return View("Index", userManager.Users);
        }

        public async Task<IActionResult> Edit(string id)
        {
            AppUser user = await userManager.FindByIdAsync(id);

            if (user != null)
            {
                return View(user);
            }
            else
            {
                return RedirectToAction("Index");
            }
        }

        [HttpPost]
        public async Task<IActionResult> Edit(string id, string username, string email, string password)
        {
            AppUser user = await userManager.FindByIdAsync(id);

            if (user != null)
            {
                user.UserName = username;
                user.Email = email;
                IdentityResult validEmail = await userValidator.ValidateAsync(userManager, user);

                if (!validEmail.Succeeded)
                {
                    AddErrorsFromResult(validEmail);
                }

                IdentityResult validPass = null;

                if (!string.IsNullOrEmpty(password))
                {
                    validPass = await passwordValidator.ValidateAsync(userManager, user, password);
                    if (validPass.Succeeded)
                    {
                        user.PasswordHash = passwordHasher.HashPassword(user, password);
                    }
                    else
                    {
                        AddErrorsFromResult(validPass);
                    }
                }

                if ((validEmail.Succeeded && validPass == null)
                    || (validEmail.Succeeded && password != string.Empty && validPass.Succeeded))
                {
                    IdentityResult result = await userManager.UpdateAsync(user);
                    if (result.Succeeded)
                    {
                        TempData["Message"] = $"Данные пользователя {user.UserName} успешно обновлены";
                        return RedirectToAction("Index");
                    }
                    else
                    {
                        AddErrorsFromResult(result);
                    }
                }
            }
            else
            {
                ModelState.AddModelError("", "Пользователь не найден");
            }

            return View(user);
        }

        private void AddErrorsFromResult(IdentityResult result)
        {
            foreach (var error in result.Errors)
            {
                ModelState.AddModelError("", error.Description);
            }
        }
    }
}

```
## Experience
* EPAM .NET Development, 2021
* ASP.NET Core MVC by A. Freeman book, Self-education

## Languages
* Belorussian (native)
* Russian (native)
* English (B1)