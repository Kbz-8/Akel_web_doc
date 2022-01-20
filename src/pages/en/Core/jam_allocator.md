---
title: JamAllocator
description: Core's memory system documentation
layout: ../../../layouts/MainLayout.astro
---

## About
___

The JamAllocator is a memory optimisation oriented allocator. It is based on the Pool Allocator model but has the particularity of being able to allocate objects like the C++ "new" allocator. Like many game engine allocators, it is based on a main allocation (the allocator heap) and the allocation requested by the user will take bits of this heap. The JamAllocator uses a system of flags to describe pruned memory areas and differentiate them from the "pure" area of the heap (the one that has not yet been allocated). Flags are 16-byte chunks allocated just before the pointer returned to the user. They contain the offset of the pointer in the heap and the size of the memory area that the pointer occupies so that when the pointer is freed, the allocator can know if it can reallocate that memory area for a next pointer.

Akel has an internal safety feature that allows the heap to be freed of all instantiated JamAllocators and FixedAllocators when a FATAL_ERROR is sent to the logs and to the kernel's error core system (which will cause the program to be stopped in an emergency).

<p align="center">
    <img src="https://raw.githubusercontent.com/SpinWaves/Akel/main/Ressources/assets/jam_alloc_diagram.png" alt="drawing" width="80%"/>
</p>

## Syntax
___
```cpp
class CustomComponent : public Ak::Component
{
    private:
        MyClass* ptr = nullptr;
        Ak::JamAllocator allocator;

    public:
        void CustomComponent::onAttach() override
        {
            allocator.init(255/*bytes*/);
            ptr = allocator.alloc<MyClass>(/* Args to pass to MyClass constructor */);
            
            Ak::debugPrint(allocator.contains((void*)ptr)) // prints "1" for true in blue
        }

        void CustomComponent::onQuit() override
        {
            allocator.free(ptr); // free the pointer
            allocator.destroy();
        }
};
```

## Member functions
___

Return type | Function | Specifiers
----------- | -------- | ----------
`[None]`    | <a href="#constructor">JamAllocator()</a> | `explicit`
`[None]`    | <a href="#destructor">~JamAllocator()</a> | `[None]`
`void`      | <a href="#initsize_t-size">init(size_t size)</a> | `[None]`
`bool`      | <a href="#canholdsize_t-size">canHold(size_t size)</a> | `inline - constexpr - noexcept`
`void`      | <a href="#auto_increase_sizebool-set">auto_increase_size(bool set)</a> | `inline - constexpr - noexcept`
`bool`      | <a href="#containsvoid-ptr">contains(void* ptr)</a> | `inline - constexpr - noexcept`
`bool`      | <a href="#is_init">is_init()</a> | `inline - constexpr - noexcept`
`void`      | <a href="#increase_sizesize_t-size">increase_size(size_t size)</a> | `[None]`
`void`      | <a href="#destroy">destroy()</a> | `[None]`
`T*`        | <a href="#allocargs--args">alloc(Args&& ... args)</a> | `[None]`
`void`      | <a href="#freet-ptr">free(T* ptr)</a> | `[None]`

### Constructor

Declaration
```cpp
explicit JamAllocator();
```

### Destructor

Declaration
```cpp
~JamAllocator();
```

### init(size_t size)

Declaration
```cpp
void init(size_t Size);
```

### canHold(size_t size)

Declaration
```cpp
inline constexpr bool canHold(size_t Size) noexcept;
```

### auto_increase_size(bool set)

Declaration
```cpp
inline constexpr void auto_increase_size(bool set) noexcept;
```

### contains(void* ptr)

Declaration
```cpp
inline constexpr bool contains(void* ptr) noexcept;
```

### is_init()

Declaration
```cpp
inline constexpr bool is_init() noexcept;
```

### increase_size(size_t size)

Declaration
```cpp
void increase_size(size_t Size);
```

### destroy()

Declaration
```cpp
void destroy();
```

### alloc(Args&& ... args)

Declaration
```cpp
template <typename T = void, typename ... Args>
T* alloc(Args&& ... args);
```

### free(T* ptr)

Declaration
```cpp
template <typename T = void>
void free(T* ptr);
```
