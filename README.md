# MINI-PROJEK-4-KOPER-MAKMUR-HISKYA
HISKYA HARSYAL KILA 2309116089


~~~

class Node:
    def __init__(self, id_koper, nama_koper):
        self.id = id_koper
        self.nama = nama_koper
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def __len__(self):
        count = 0
        current = self.head
        while current:
            count += 1
            current = current.next
        return count

    def tambah_awal(self, id_koper, nama_koper):
        new_node = Node(id_koper, nama_koper)
        new_node.next = self.head
        self.head = new_node

    def tambah_akhir(self, id_koper, nama_koper):
        new_node = Node(id_koper, nama_koper)
        if self.head is None:
            self.head = new_node
            return
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    def tambah_tengah(self, id_koper, nama_koper, id_sebelum):
        new_node = Node(id_koper, nama_koper)
        if self.head is None:
            self.head = new_node
            return
        current = self.head
        while current.next and current.id != id_sebelum:
            current = current.next
        if current.id == id_sebelum:
            new_node.next = current.next
            current.next = new_node
        else:
            print("ID sebelum tidak ditemukan")

    def hapus_awal(self):
        if self.head is None:
            print("Linked List kosong")
            return
        self.head = self.head.next

    def hapus_akhir(self):
        if self.head is None:
            print("Linked List kosong")
            return
        if self.head.next is None:
            self.head = None
            return
        current = self.head
        while current.next.next:
            current = current.next
        current.next = None

    def hapus_tengah(self, id_koper):
        if self.head is None:
            print("Linked List kosong")
            return
        if self.head.id == id_koper:
            self.head = self.head.next
            return
        current = self.head
        while current.next and current.next.id != id_koper:
            current = current.next
        if current.next is None:
            print("ID koper tidak ditemukan")
        else:
            current.next = current.next.next

    def merge_sort(self, mode='asc', key='id'):
        if self.head is None:
            return
        self.head = self._merge_sort_rec(self.head, mode, key)

    def _merge_sort_rec(self, node, mode, key):
        if node.next is None:
            return node
        middle = self._get_middle(node)
        next_to_middle = middle.next
        middle.next = None
        left = self._merge_sort_rec(node, mode, key)
        right = self._merge_sort_rec(next_to_middle, mode, key)
        sorted_list = self._merge(left, right, mode, key)
        return sorted_list

    def _get_middle(self, node):
        if node is None:
            return node
        slow = node
        fast = node
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def _merge(self, left, right, mode, key):
        result = None
        if mode == 'asc':
            if key == 'id':
                result = self._merge_sorted_by_id_asc(left, right)
            else:
                result = self._merge_sorted_by_nama_asc(left, right)
        else:
            if key == 'id':
                result = self._merge_sorted_by_id_desc(left, right)
            else:
                result = self._merge_sorted_by_nama_desc(left, right)
        return result

    def _merge_sorted_by_id_asc(self, left, right):
        result = None
        if left is None:
            return right
        if right is None:
            return left
        if left.id <= right.id:
            result = left
            result.next = self._merge_sorted_by_id_asc(left.next, right)
        else:
            result = right
            result.next = self._merge_sorted_by_id_asc(left, right.next)
        return result

    def _merge_sorted_by_nama_asc(self, left, right):
        result = None
        if left is None:
            return right
        if right is None:
            return left
        if left.nama.lower() <= right.nama.lower():
            result = left
            result.next = self._merge_sorted_by_nama_asc(left.next, right)
        else:
            result = right
            result.next = self._merge_sorted_by_nama_asc(left, right.next)
        return result

    def _merge_sorted_by_id_desc(self, left, right):
        result = None
        if left is None:
            return right
        if right is None:
            return left
        if left.id >= right.id:
            result = left
            result.next = self._merge_sorted_by_id_desc(left.next, right)
        else:
            result = right
            result.next = self._merge_sorted_by_id_desc(left, right.next)
        return result

    def _merge_sorted_by_nama_desc(self, left, right):
        result = None
        if left is None:
            return right
        if right is None:
            return left
        if left.nama.lower() >= right.nama.lower():
            result = left
            result.next = self._merge_sorted_by_nama_desc(left.next, right)
        else:
            result = right
            result.next = self._merge_sorted_by_nama_desc(left, right.next)
        return result

    def jump_search_by_id(self, id_koper):
        if self.head is None:
            print("Linked List kosong")
            return
        n = len(self)
        prev = 0
        step = int(n ** 0.5)
        while self.head:
            node = self.node_at_index(step)
            if node is None:
                break
            if node.id == id_koper:
                print(f"Koper dengan ID {id_koper} ditemukan: {node.nama}")
                return
            elif node.id > id_koper:
                prev = step
                break
            prev = step
            step += int(n ** 0.5)
        node = self.head
        while node:
            if node.id == id_koper:
                print(f"Koper dengan ID {id_koper} ditemukan: {node.nama}")
                return
            node = node.next
        print(f"Koper dengan ID {id_koper} tidak ditemukan")

    def jump_search_by_nama(self, nama_koper):
        if self.head is None:
            print("Linked List kosong")
            return
        n = len(self)
        prev = 0
        step = int(n ** 0.5)
        while self.head:
            node = self.node_at_index(step)
            if node is None:
                break
            if node.nama.lower() == nama_koper.lower():
                print(f"Koper dengan nama {nama_koper} ditemukan: ID {node.id}")
                return
            elif node.nama.lower() > nama_koper.lower():
                prev = step
                break
            prev = step
            step += int(n ** 0.5)
        node = self.head
        while node:
            if node.nama.lower() == nama_koper.lower():
                print(f"Koper dengan Nama {nama_koper} ditemukan: Dengan ID {node.id}")
                return
            node = node.next
        print(f"Koper dengan Nama {nama_koper} tidak ditemukan")

    def node_at_index(self, index):
        if self.head is None:
            return None
        current = self.head
        for _ in range(index):
            if current.next:
                current = current.next
            else:
                return None
        return current

    def _len_(self):
        count = 0
        current = self.head
        while current:
            count += 1
            current = current.next
        return count

    def print_list(self):
        current = self.head
        while current:
            print(f"ID: {current.id}, Nama: {current.nama}")
            current = current.next

# Buat linked list
linked_list = LinkedList()

# Tambahkan koper awal
linked_list.tambah_awal(13, "Baller")
linked_list.tambah_awal(23, "Roaming")
linked_list.tambah_awal(33, "Lojel")
linked_list.tambah_awal(43, "Airwheel")
linked_list.tambah_awal(53, "Samsonite")
linked_list.tambah_awal(63, "Tumi")
linked_list.tambah_awal(73, "Kamiliant")
linked_list.tambah_awal(83, "Delsey")
linked_list.tambah_awal(93, "Eminent")

while True:
    print("\n=======||Koper Makmur||=======")
    print("1. Tampilkan daftar koper")
    print("2. Tambah koper di awal")
    print("3. Tambah koper di akhir")
    print("4. Tambah koper di tengah")
    print("5. Hapus koper di awal")
    print("6. Hapus koper di akhir")
    print("7. Hapus koper di tengah")
    print("8. Urutkan koper (Merge Sort)")
    print("9. Cari koper (Jump Search)")
    print("10. Keluar")
    print("="*29)

    pilihan = input("Masukkan pilihan: ")

    if pilihan == "1":
        linked_list.print_list()

    elif pilihan == "2":
        id_koper = int(input("Masukkan ID koper: "))
        nama_koper = input("Masukkan Nama koper: ")
        linked_list.tambah_awal(id_koper, nama_koper)
        print("Koper berhasil ditambahkan di awal.")

    elif pilihan == "3":
        id_koper = int(input("Masukkan ID koper: "))
        nama_koper = input("Masukkan Nama koper: ")
        linked_list.tambah_akhir(id_koper, nama_koper)
        print("Koper berhasil ditambahkan di akhir.")

    elif pilihan == "4":
        id_koper = int(input("Masukkan ID koper: "))
        nama_koper = input("Masukkan Nama koper: ")
        id_sebelum = int(input("Masukkan ID koper sebelumnya: "))
        linked_list.tambah_tengah(id_koper, nama_koper, id_sebelum)
        print("Koper berhasil ditambahkan di tengah.")

    elif pilihan == "5":
        linked_list.hapus_awal()
        print("Koper di awal berhasil dihapus.")

    elif pilihan == "6":
        linked_list.hapus_akhir()
        print("Koper di akhir berhasil dihapus.")

    elif pilihan == "7":
        id_koper = int(input("Masukkan ID koper yang akan dihapus: "))
        linked_list.hapus_tengah(id_koper)
        print("Koper di tengah berhasil dihapus.")

    elif pilihan == "8":
        mode = input("Masukkan mode sorting (asc/desc): ")
        key = input("Masukkan kunci sorting (id/nama): ")
        linked_list.merge_sort(mode, key)
        print("Daftar koper berhasil diurutkan.")

    elif pilihan == "9":
        metode = input("Masukkan metode pencarian (id/nama): ")
        if metode == "id":
            id_koper = int(input("Masukkan ID koper yang dicari: "))
            linked_list.jump_search_by_id(id_koper)
        elif metode == "nama":
            nama_koper = input("Masukkan Nama koper yang dicari: ")
            linked_list.jump_search_by_nama(nama_koper)
        else:
            print("Metode pencarian tidak valid.")

    elif pilihan == "10":
        print("Program Berakhir, Terima kasih.")
        break

    else:
        print("Pilihan tidak valid. Silakan coba lagi.")

~~~
