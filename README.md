# Invalid Material Handler  
A utility for managing invalid materials in Call of Duty: Black Ops 2 on Xbox 360.

---

## Why Share?  
Let’s face it—people love to share code without fully understanding it, or they act like it’s some closely guarded secret. So, I’m releasing it for everyone.

The code may be messy, but it gets the job done. I’ve also included the necessary hook addresses. While this is designed for Black Ops 2, you can easily adapt it for other games with similar invalid material issues.

---

## Extra
Good job Sunset Developer Matthew, Imagine pasting code that you didn't know was useless It's how i found out you got passed my code. Clap clap clap.

---

### Hooks & Addresses
```cpp
SEH_ReadCharFromString - 0x82443A80
LUI_CoD_ReadCharFromString - 0x82717AE0

int SEH_ReadCharFromStringHook(const char** textPtr, unsigned int* letter, const Vector4* originalColor, color_t* color, char** buttonName, Material** iconMaterial) {
		if (doesTextContainInvalidMaterial(*textPtr))
		{
			*textPtr = "Blocked this nerds crash";
		}
	return SEH_ReadCharFromStringStub(textPtr, letter, originalColor, color, buttonName, iconMaterial);
}

bool LUI_CoD_ReadCharFromStringHook(const char** textPtr, unsigned int* letter, const Vector4* originalColor, color_t* color, char** buttonName, Material** iconMaterial) {
		if (doesTextContainInvalidMaterial(*textPtr))
		{
			*textPtr = "Blocked this nerds crash";
		}
	return LUI_CoD_ReadCharFromStringStub(textPtr, letter, originalColor, color, buttonName, iconMaterial);
}
```

### Handler Code

```cpp
bool isValidMaterial(Material* material)
{
	return material != 0;
}

bool isValidMaterialName(const char* name)
{
	int len = strlen(name);

	if (len > 1)
	{
		char kek = name[1];

		switch (kek)
		{
		case('H'):
		case('$'):
		case('H=='):
		case('I'):
		{
			if (len > 5)
			{
				char size = name[4];
				const char* material = &name[5];

				if (strlen(material) < size)
				{
					return false;
				}

				if (!isValidMaterial(Material_RegisterHandle(material, 7)))
				{
					return false;
				}
			}
			else
			{
				return false;
			}

			break;
		}
		case('B'):
		{
			bool shitFound = false;
			const char* start = name + 2;

			while (*start)
			{
				if (*start++ == '^')
					shitFound = true;
			}

			if (!shitFound)
				return false;

			break;
		}
		}
	}

	return true;
}

bool doesTextContainInvalidMaterial(const char* text) {
	if (!text)
	{
		return true;
	}

	const char* pos = text;
	while (*pos)
	{
		if (*pos == '^')
		{
			if (!isValidMaterialName(pos))
			{
				return pos;
			}
		}
		pos++;
	}

	return false;
}
```
---

## Credits
- Heaventh
- My Crow Soft
- Jordywastaken
- Kiwi_Modz
- Faultz
- Execcl
- Synful

These individuals have turned my C++ journey into an unforgettable adventure—equal parts exhilarating and frustrating.


## Contributions
Anyone is welcome to create pull requests if they really want. I don't really mind.
